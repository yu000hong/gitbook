# dict vs collections.abc.Mapping

[What does isinstance with a dictionary and abc.Mapping from collections doing?](https://stackoverflow.com/questions/35690572/what-does-isinstance-with-a-dictionary-and-abc-mapping-from-collections-doing)

### Question

The code I'm running is:

```python
>>> from collections import abc
>>> mydict = {'test_key': 'test_value'}
>>> isinstance(mydict, abc.Mapping)
True
>>> isinstance(mydict, dict)
True
```

I understand what `isinstance()`does, but I'm not sure what `abc.Mapping` does from collections?

It seems like the line `isinstance(mydict, abc.Mapping)` is being used to check that **mydict** is a dictionary?
Wouldn't it just be easier to do `isinstance(mydict, dict)`?

### Answer

`collections.abc` provide a serie of Abstract Base Classes for container. The `collections.abc` module provides several abstract base classes that can be used to generically describe the various kinds of data structures in Python. In your example, you test if an object is an instance of the `Mapping` abstract class, which will be true for many classes that "work like a dictionary" (e.g. they have a `__getitem__()` method that takes hashable keys and return values, have keys, values and items methods, etc.). Such dict-like objects might inherit from dict but they don't need to.

The abstract types in `collections.abc` are implemented using the top level `abc` module. `dict` is registered as a `MutableMapping`(which is a subclass of `Mapping`) and so the `isinstance()` check will accept a dictionary as a `Mapping` even though `Mapping` isn't an actual base class for `dict`.
