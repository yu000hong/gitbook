# Class attribute vs Object attribute

```python
class Person:
	name = 'hello'

	def __init__(self, age):
		self.age = age
```

**name** is class attribute, and **age** is object attribute.

```python
p = Person(18)
```

Here, **p** is an object of Person, we can get attributes use `p.age` & `p.name`.

If object **p** has no attribute of **name**, python will search its class attribute.

So, `p.name` will return 'hello'.

If we set object attriute name, `p.name = 'yu000hong'`, next time `p.name` will return 'yu000hong' from its object attribute, python will not search its class attribute.

Object attribute has no relationship with class attribute, so if we set class attribute, it will not affect all object attributes. Every object has its own object attribute, and they won't affect each other.

```python
Person.name = 'rex'
p2 = Person(18)
p2.name  # rex
```

## Notice the difference

```python
class Person2:
	name: str

	def __init__(self, age):
		self.age = age
```

Person has class attribute name, but Person2 has not! Please notice the difference of this two classes!

