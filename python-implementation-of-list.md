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





