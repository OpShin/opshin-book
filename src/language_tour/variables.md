# Variables

Variables in Opshin are declared just like you'd expect them to in Python:

```python
# A simple variable declaration
x = 5 

# A variable declaration with annotated type, x must be an integer
x: int = 5 

# Variables in Opshin can be mutated.
x += 1
```

>**Note:** For now `int` is the only number type available in Opshin. `fractions` are coming soon.

#### Type annotation

If you want to make sure that a variable has the type you expect it to have
you may use type annotation at the time of assinging a variable.

```python
x: int = some_function()
```

This will make sure that `some_function` actually returns an integer.

Annotations can also be used for type up- and downcasts.
For example if you receive a value `y` of type `Union[A, B]` you can run

```python
z: A = y
```

This will cast `y` to type `A` in the type system but will not check the type
during runtime. Use with care.

The other way around, if you receive a value of type `A` but you may want
to use it as a `Union[A, B]` you can run

```python
z: Union[A, B] = y
```

This will allow you to also store objects of type `B` in `z` later in the code.

> Note that the type of a variable can not be changed after initialization.
> This is true as of version 0.19.0 and may change again in a later version.

## Tuple Assignments

Opshin supports Python's tuple assignment syntax:

```python
a, b = 3, 7
```
