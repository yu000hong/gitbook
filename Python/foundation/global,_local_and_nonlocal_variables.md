# Global, Local and Nonlocal variables

## Global Variables

In Python, a variable declared outside of the function or in global scope is known as a global variable. This means that a global variable can be accessed inside or outside of the function.

```python
x = "global"

def foo():
    print("x inside:", x)

foo()
print("x outside:", x)
```

**Output:**

```
x inside: global
x outside: global
```

What if you want to change the value of `x` inside a function?

```python
x = "global"

def foo():
    x = x * 2
    print(x)

foo()
```

**Output:**

```
UnboundLocalError: local variable 'x' referenced before assignment
```

The output shows an error because Python treats `x` as a local variable and `x` is also not defined inside `foo()`.

To make this work, we use the `global` keyword.

## Nonlocal Variables

Nonlocal variables are used in **nested functions** whose local scope is not defined. This means that the variable can be neither in the local nor the global scope.

```python
def outer():
    x = "local"

    def inner():
        nonlocal x
        x = "nonlocal"
        print("inner:", x)

    inner()
    print("outer:", x)

outer()
```

**Output:**

```
inner: nonlocal
outer: nonlocal
```




