# ValueError vs TypeError

A ValueError is

>  when a built-in operation or function receives an argument that has the right type but an inappropriate value.

The `float()` function can take a string, ie float('5'), it's just that the value 'string' in `float('string')` is an inappropriate (non-convertible) string.

On the other hand,

> Passing arguments of the wrong type (e.g. passing a list when an int is expected) should result in a TypeError. TypeError can also occur, when we pass incorrect number of arguments to a function.

So you would get a `TypeError` if you tried `float(['5'])` because a list can never be converted into a float.


Ok, here's a couple of really easy examples:

```python
len(42)      # this causes a TypeError, because a number is the wrong type for the function
int("dog")   # this causes a ValueError, because "dog" cannot be converted to an int value
```

