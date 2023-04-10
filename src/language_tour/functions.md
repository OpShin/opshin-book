# Functions

Functions in Opshin are just like in Python:

## Regular Functions

Functions in Opshin should have type annotations for the arguments and the return.
There must be one `return` statement at the end of a function.
If it is omitted, the function returns `None`.
Note it is not possible to return from anywhere else but the end of the function.

```python
def fibonacci(n: int) -> int:
    if n < 2:
        res = 1
    else:
        res = fibonacci(n-1) + fibonacci(n-2)
    return res
```

As shown, Opshin functions support recursion. 

## Lambda and Local Functions

Generally OpShin discourages the use of lambda functions, as they don't allow specifying types in the function signature.
Instead, consider using a local function definitions or list expressions. For example instead of the following expression

```python
ys = map(lambda x: 2*x, filter(lambda x: x > 0, xs))
```

You could either define local functions like this

```python
def greater_zero(x: int) -> bool:
  return x > 0

def mul_2(x: int) -> int:
  return 2*x

ys = map(mul_2, filter(greater_zero, xs))
```

Or you can express this very compactly and readable directly in a list expression like this

```python
ys = [2*x for x in xs if x > 0]
```

Note that you can define functions locally and within other functions, so that they do not clutter your namespace and can not be used in the wrong context.
The following is legal.

```python
def mul_twice(x: int) -> int:

   def mul_once(x: int) -> int:
        return x * x

   return mul_once(x) * mul_once(x)
```
