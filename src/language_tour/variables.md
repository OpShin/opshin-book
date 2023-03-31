# Variables

Variables in Opshin are declared just like you'd expect them to in Python:

```python
# A simple variable declaration
x: int = 5 

# Variables in Opshin can be mutated.
x += 1
```

>**Note:** For now `int` is the only number type available in Opshin. `fractions` are coming soon.

## Type Annotations

Opshin comes builtin with really good type-inference so type-annotations are optional.
The code above could be rewritten as:

```python
x = 5
```

## Tuple Assignments

Opshin supports Python's tuple assignment syntax:

```python
a, b = 3, 7
```
