## Implement List

The c structure of list

```
typedef struct {
    PyObject_VAR_HEAD
    PyObject **ob_item;
    Py_ssize_t allocated;
} PyListObject;
```

ob\_item is the reference array and allocated is the pre-assigned memory of ob\_item

### Init List

let's see what happened when initing the empty list:

```python
arguments: size of the list = 0
returns: list object = []
PyListNew:
    nbytes = size * size of global Python object = 0
    allocate new list object
    allocate list of pointers (ob_item) of size nbytes = 0
    clear ob_item
    set list's allocated var to 0 = 0 slots
    return list object
```

非常重要的是知道list申请内存空间的大小（后文用allocated代替）的大小和list实际存储元素所占空间的大小\(ob\_size\)之间的关系，ob\_size的大小和len\(L\)是一样的，而allocated的大小是在内存中已经申请空间大小。通常你会看到allocated的值要比ob\_size的值要大。这是为了避免每次有新元素加入list时都要调用realloc进行内存分配。接下来我们会看到更多关于这些的内容。

### Append

what happened when appending a new element

```python
arguments: list object, new element
returns: 0 if OK, -1 if not
app1:
    n = sisze of list
    call list_resize() to resize the list to size n+1 = 0 + 1 = 1
    list[n] = list[0] = new element
    return 0
```

list\_resize\(\) will apply more space to avoid frequent re-application. so the increasing model of list is :0,4,8,16,25,35,46,58,72,88...

![](/assets/importlistimplementation.png)

If we keep adding 2,3 ,4, we don't have to apply new space because the extra space is big enough

![](/assets/pythonlistimplementation2.png)

## Insert

let's insert 5 into index 1

![](/assets/pythonlistimplementation3.png)

you can see that the allocated space is 8 but we only used 5 space there

the time complexity of insert is O\(n\)

### Pop

when popping an element, the list\_resize function will be called

If ob\_size is less than half of the allocated space, the allocated space will be reduced

![](/assets/pythonlistimplementation4.png)

你可以发现4号内存空间指向还指向那个数值（译者注：弹出去的那个数值），但是很重要的是ob\_size现在却成了4.  
让我们再弹出一个元素。在list\_resize内部，size – 1 = 4 – 1 = 3 比allocated（已经申请的空间）的一半还要小。所以list的申请空间缩小到  
6个，list的实际使用空间现在是3个\(译者注：根据\(newsize &gt;&gt; 3\) + \(newsize &lt; 9 ? 3 : 6\) = 3在文章最后有详述\)  
你可以发现（下图）3号和4号内存空间还存储着一些整数，但是list的实际使用\(存储元素\)空间却只有3个了。





![](/assets/pythonlistimplementation5.png)





