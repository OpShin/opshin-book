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

## Lambda Functions

> Opshin currently doesn't support `lambda` functions but they're a work in progress.
> We'll update this section as soon as they're working.
