## Variable  Scope

Only when defining variables in Module, Class,def, it will generate Scope.

```
#!/usr/bin/env python
def func():
    variable = 100
    print variable
print variable
```

```
NameError: name 'variable' is not defined

```

If we define the variable in if-elif-else、for-else、while、try-except\try-finally, it doesn't generate scope.

```
if True:
    variable = 100
    print (variable)
print ("******")
print (variable)
```

```
100
******
100
```



