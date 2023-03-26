# Functions

Functions in Opshin are just like in Python:

## Regular Functions

```python
def fibonacci(n: int) -> int:
    if n < 2:
        return 1
    else:
        return fib(n-1) + fib(n-2)
```
