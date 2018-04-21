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
  
  
