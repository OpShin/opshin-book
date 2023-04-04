# Functions

Functions in Opshin are just like in Python:

## Regular Functions

Functions in Opshin must have type annotations for the arguments and the return.

```python
def fibonacci(n: int) -> int:
    if n < 2:
        return 1
    else:
        return fib(n-1) + fib(n-2)
```

## Lambda Functions (WIP)

Opshin currently doesn't support `lambda` functions but they're a work in progress.
We'll update this section as soon as they're working.
