# Container Types

Opshin has two main container types:

- `List[a]`
- `Dict[k, v]`

>**Note:** The container types are generic, and the letter in square brackets (`[]`) are the type arguments

## `List[a]`

The Opshin `List` type is a container type that is used to hold multiple values of the same type.
This is works just like the `list` type in Python.

```python
listy: List[int] = [1, 1, 2, 3, 5]
```

>**Note:** Opshin lists are actually *linked-lists*.
> This is because that's how lists are implemented in UPLC.

### List Access

You may access elements at arbitrary positions within a list like this:

```python
listy[3]  # will return the element at the 4th position (3rd with 0-based indexing), i.e. 3
```

If you want to count from the back, use negative numbers:

```python
listy[-2]  # returns the second-to-last element, i.e. 3
```

### List Comprehension

Opshin supports Python's list comprehension syntax.
This allows for very compact list initialization:

```python
squares = [x**2 for x in range(100)]
```

#### `len()`

The `len` method returns the length of the list as an `int`:

```python
lenny: int = len(listy) 
lenny == 5  # True
```

## `Dict[k, v]`

The `Dict` type represents a hashmap that maps keys of type `k` to values of type `v`.
It works just like the `dict` type in Python.

```python
# A dictionary storing scores in a game.
scores: Dict[str, int] = {"god_binder": 12, "radio_knight": 42}
```


#### `.keys()`

The `.keys()` method returns a list of the keys in a dictionary,

```python
players: List[str] = scores.keys() # ["god_binder", "radio_knight"]
```

#### `.values()`

The `.values()` method

```python
raw_scores: List[int] = scores.values() # [12, 42]
```

#### `.items()`


The `.items()` method returns a tuple of the each key-value pair in the dictionary.
This is particularly useful if you want to iterate over all pairs contained in a dictionary.

```python
for username, score in scores.values():
    print(username + "scored:" + str(score))
```
