# Container Types

Opshin has two main container types:

- `list[a]`
- `Dict[k, v]`

>**Note:** The container types are generic, and the letter in square brackets (`[]`) are the type arguments

## `list[a]`

The Opshin `list` type is a container type that is used to hold multiple values of the same type.
This is works just like the `list` type in Python.

```python
listy: list[int] = [1, 1, 2, 3, 5]
```

>**Note:** Opshin lists are actually *linked-lists*.
> This is because that's how lists are implemented in UPLC.

### Useful Methods

#### `.len()`

The `.len()` method returns the length of the list as an `int`:

```python
lenny: int = listy.len() 
lenny == 5  # true
```

## `Dict[k, v]`

The `Dict` type represents a hashmap that maps keys of type `k` to values of type `v`.
It works just like the `dict` type in Python.

```python
# A dictionary storing scores in a game.
scores: Dict[str, int] = {"god_binder": 12, "radio_knight": 42}
```

### Useful Methods

#### `.keys()`

The `.keys()` method returns a list of the keys in a dictionary,

```python
players: list[str] = scores.keys() # ["god_binder", "radio_knight"]
```

#### `.values()`

The `.values()` method

```python
raw_scores: list[str] = scores.values() # [12, 42]
```

#### `.items()`

the `.items()` method returns a tuple of the each key-value pair in the dictionary.

```python
for (username, score) in scores.values():
    print(username, "scored:", score)
```
