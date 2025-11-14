# Container Types

Opshin has two main container types, lists and dicts:

- `List[a]`
- `Dict[k, v]`

>**Note:** The container types are generic, and the letter in square brackets (`[]`) are the type arguments

They come in handy if you want to handle large amounts of values of the same type.

## `List[a]`

The Opshin `List` type is a container type that is used to hold multiple values of the same type.
This is works just like the `list` type in Python. In particular, you simply place all values in the desired order into the list.
For example, for a list of integers from 1 to 5, you can write the following:

```python
listy: List[int] = [1, 2, 3, 4, 5]
```

If you omit the type annotation, OpShin will attempt to derive the most precise type that applies to all values in the list.
For example, if you specify `[1, "hellooo"]`, it will derive the type `List[Union[str, int]]`.

### List Operations

You can add lists using the `+` operator.

```python
print([1, 2, 3, 4] + [5, 6])
# prints "[1, 2, 3, 4, 5, 6]"
```

### List Access

You may access elements at arbitrary positions within a list like this:

```python
listy[3]  # will return the element at the 4th position (3rd with 0-based indexing), i.e. 4
```

If you want to count from the back, use negative numbers:

```python
listy[-2]  # returns the second-to-last element, i.e. 4
```
>**Note:** In contrast to Python lists, which have constant time index access, Opshin lists are actually *linked-lists* and access time is linear to their size.
> This is because that's how lists are implemented in UPLC.

### List Slices

You may access slices of a list using the slicing syntax.

```python
["a", "b", "c", "d"][1:3]  # returns ["b", "c"]
```


### List Comprehension

Opshin supports Python's list comprehension syntax.
This allows for very compact list initialization:

```python
squares = [x**2 for x in listy]
```

#### `len(x)`

The `len` method returns the length of the list as an `int`:

```python
lenny: int = len(listy) 
lenny == 5  # True
```

#### Membership using `in`

You can check whether some element is included in a list of elements using the keyword `in`.

```python
4 in [1, 2, 3, 4, 5]  # True
100 in range(10)  # False
```

#### Empty lists

Since OpShin will try to infer the type of the list automatically based on its content, empty lists pose a special challenge.
If you omit the type annotation on an empty list literal, it will be assigned type `List[Anything]`.
That means you can not do any sensible operations on values in the list.
If you need an empty list of a specific type, initialize it in a typed assignment.

```python
empty_list_any = []  # has type List[Anything]
empty_list_any + [1]  # Fails type check: can not concatenate list of type int and Anything
empty_list_int: List[int] = []  # has type List[int]
empty_list_int + [1]  # works fine
```


## `Dict[k, v]`

The `Dict` type represents a map from keys of type `k` to values of type `v`.
It works just like the `dict` type in Python.

```python
# A dictionary storing scores in a game.
scores: Dict[str, int] = {"god_binder": 12, "radio_knight": 42}
```

Just as for lists, OpShin automatically derives precise key and value types.

### Dictionary Access

A dictionary implements a map. In order to find the mapped value of element `x` we can
access it in the dictionary via `dict[x]`.

```python
scores["god_binder"]  # returns 12
```

Take care when accessing values that are not contained in the dictionary - 
in case of missing keys, the contract will fail immediately.

```python
scores["peter_pan"]  # fails with "KeyError"
```

If you are not sure whether a key maps to something in the dictionary
use `dict.get(x, d)`. It will try to return the value mapped to by `x` in the dictionary.
If `x` is not present it will return `d`.

> It is important that `d` is of the value type `v` to guarantee type safety.

```python
scores.get("god_binder", 0)  # returns 12
scores.get("peter_pan", 0)  # returns 0
```

> **Note:** Random access to dictionaries in OpShin has cost linear in the size of the dictionary instead of constant, due to the nature of the underlying target language UPLC.

#### `.keys()`

The `.keys()` method returns a list of the keys in a dictionary,

```python
players: List[str] = scores.keys() # ["god_binder", "radio_knight"]
```

#### `.values()`

The `.values()` method returns a list of all the values in a dictionary.

```python
raw_scores: List[int] = scores.values() # [12, 42]
```

#### `.items()`


The `.items()` method returns a tuple of the each key-value pair in the dictionary.
This is particularly useful if you want to iterate over all pairs contained in a dictionary.

```python
for username, score in scores.items():
    print(f"{username} scored: {score}")
    # prints first "god_binder scored: 12" and then "radio_knight scored: 42"
```

#### Empty dicts

As with lists, empty dictionaries pose a special challenge.
If you omit the type annotation on an empty dictionary literal, it will be assigned type `Dict[Anything, Anything]`.
That means you can not do any sensible operations on values in the dict, and `.get()` will return values of type `Anything`.
If you need an empty dictionary of a specific type, initialize it in a typed assignment.

```python
empty_dict_any = {}  # has type Dict[Anything, Anything]
empty_dict_any.get("hi", 1) + 2  # Fails type check: can not add types int and Anything
empty_dict_int: Dict[str, int] = {}  # has type Dict[str, int]
empty_dict_int.get("hi", 1) + 2  # works!
```
