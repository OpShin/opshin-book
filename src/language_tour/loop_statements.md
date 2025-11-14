# Loop Statements

Opsin supports both `while` and `for` loops.

## `while` loops

`while` loops in Opshin work just like in Python.
They have a body and a boolean expression and the code in the body is looped *while* the boolean expression returns `True`.

```python
count = 0

while count < 5:
    print("'count' is ", count, ".")
    count += 1
```

## `for` loops

`for` loops are use for iterating over lists and other iterables.
The loop through the elements of the iterable and executes the code in the block using the element from the list.

```python
names = ["Ada", "Voltaire", "Basho"]
for name in names:
    print(name)

for i in range(10):
    ...
```

Dictionaries are also iterables and they can be iterated over using a `for` loop:

```python
scores = {"Byron": 1200, "Vasil": 900, "Ada": 1790, "Shelley": 1400}

# Iterating over the keys and values using '.items()'
for name, score in scores.items():
    print(name + " scored: " + str(score))

# Iterating over the keys using '.keys()'
for name in scores.keys():
    ...

# Iterating over the values using '.values()'
for score in scores.values():
    ...
```
