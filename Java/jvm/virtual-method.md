### Virtual Method

In object-oriented programming, a virtual function or virtual method is a function or method whose behavior can be overridden within an inheriting class by a function with the same signature. This concept is a very important part of the polymorphism portion of object-oriented programming (OOP).

Virtual Methods are methods in Java that are non-static and without the keyword final in front. All methods by default are virtual in Java. Virtual Methods play important roles in Polymorphism because children classes in Java can override their parent classes' methods if the function being overriden is non-static and has the same method signature.

There are, however, some methods that are not virtual. For example, if the method is declared private or with the keyword final, then the method is not Virtual.

Additionally, we can implement Virtual methods using the abstract keyword. Methods declared with the keyword "abstract" does not have a method definition, meaning the method's body is not yet implemented.

-  A virtual function cannot be private as we cannot override private methods of the parent class.
- A virtual function cannot be stated as Final, as we cannot override Final methods.
- A virtual function cannot be stated as Static, as we cannot override static methods.

- You can write virtual functions in Java with interfaces.

> Java interface methods are all "pure virtual" because they are designed to be overridden.

- You can write virtual functions in Java with abstract classes.

> Java Abstract classes contain implicitly "virtual" methods, implemented by classes extending it.

- You can force a function to NOT be virtual in a generic class by making it `final`