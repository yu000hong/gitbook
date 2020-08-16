# hasattr(), getattr(), setattr()

> 这三个方法是针对对象的，而非dict

```python
d = {'name': 'yuhong'}
hasattr(d, 'name')  # false
```

## getattr()

如果对象没有对应的属性的话， 会抛异常`AttributeError`！

## hasattr()

`hasattr()`的实现就是利用`getattr()`方法，如果不抛异常，那么说明包含此属性；否则不包含。

## setattr()

`setattr()`可以给对象属性赋值，属性可以是已经存在的属性，也可以是不存在的新属性。

但是，某些对象使用setattr()设置新属性会抛异常`AttributeError`！如：

- object,
- int,
- str,
- float
- dict,
- tuple,
- list
- set
- …

> **特别注意:** object对象设置新属性也会抛异常，但是继承自object的对象就可以设置新属性！

```python
a = object()
setattr(a, 'name', 'yh')   # AttributeError: 'object' object has no attribute 'name'

class Person:
	name = ''

p = Person()

setattr(p, 'age', 16)  # success
p.age   # 16
```
