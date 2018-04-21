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