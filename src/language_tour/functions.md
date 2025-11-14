# Functions

## Regular Functions

Functions in Opshin should have type annotations for the arguments and the return value.
Return statements can be placed anywhere inside a function.
Note that all return statements must have a type compatible with the annotated
return type.

```python
def fibonacci(n: int) -> int:
    if n < 2:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```

A function `foo` is called as `foo(x, y, ...)` with arguments `x`, `y`, etc.
As shown, Opshin functions support recursion, i.e. the function `fibonacci` is visible inside itself and can be called from within itself.

When there are several arguments, you can use the name of the argument to specify the argument during calling.
This allows for cleaner code and makes code robust to changes to the function signature.

```python
def foo(x: int, y:int, z:int) -> int:
    return (x + y) * z

# you can specify the name of the argument to make clear which number is which argument
assert foo(x=1, y=2, z=3) == foo(1, 2, 3)
# you can also change the order and provide keywords only partially
assert foo(z=3, x=1, y=2) == foo(1, 2, z=3)
```

If the function does not have a return statement in some path, the implicit return value is `None`.
This can be useful for code that has side-effects, such as assertion checks.

```python
def check_valid(n: int) -> None:
  assert n > 0, f"Invalid negative int encountered: {n}"
```

If the type annotation is missing for any field, the implicit annotation is `Any`.
This may be fine for your use case, but note that this is slightly less efficient (and less clear to the reader) than a properly type annotated function.

Note that you can define functions locally and within other functions, so that they do not clutter your namespace and can not be used in the wrong context.
The following is legal.

```python
def mul_twice(x: int) -> int:

   def mul_once(x: int) -> int:
        return x * x

   return mul_once(x) * mul_once(x)

print(mul_twice(5))
# "625"
```

## Lambdas and list expressions

Generally OpShin does not support the use of lambda functions, as they don't allow specifying types in the function signature.
Instead, consider using a local function definitions or list expressions. For example instead of the following expression

```python
ys = map(lambda x: 2*x, filter(lambda x: x > 0, xs)) # does not work.
```

You could either define local functions like this

```python
def greater_zero(x: int) -> bool:
  return x > 0

def mul_2(x: int) -> int:
  return 2*x

ys = map(mul_2, filter(greater_zero, xs)) # does not work.
```

Or you can express this very compactly and readable directly in a list expression like this

```python
ys = [2*x for x in xs if x > 0] # works!
```

