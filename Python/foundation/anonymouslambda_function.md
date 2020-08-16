# Anonymous/Lambda Function

In Python, an anonymous function is a function that is defined without a name.

While normal functions are defined using the `def` keyword in Python, anonymous functions are defined using the `lambda` keyword.

Hence, anonymous functions are also called lambda functions.

### Syntax

```python
lambda arguments: expression
```

Lambda functions can have any number of arguments but only one expression. The expression is evaluated and returned. Lambda functions can be used wherever function objects are required.

### Sample

```python
# Program to show the use of lambda functions
double = lambda x: x * 2

print(double(5))
```

**Output:**

```
10
```

