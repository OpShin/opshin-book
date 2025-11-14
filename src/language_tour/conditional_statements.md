# Conditional Statements

Opshin uses `if`, `elif` and `else` statements for conditional control flow.
`elif` is a short form of `else if` and behaves like you would expect it.

## `if`

```python
n = 4
if n < 5:
    print("Less than 5.")
elif n == 5:
    print("Equal to 5.")
else:
    print("Greater than 5.")
```

## `pass`

This statement does not do anything.
It is mainly used to show the compiler that you respect an indent for an otherwise
empty control flow branch.

```python
if check:
    pass
else:
    some_important_function()
```

## `assert`

The assert statement is an important tool in Smart Contracts to check
that conditions of the contract are met.
It checks that a statement is correct and otherwise immediately halts the contract
with an error message if provided.

```python
assert money_locked_in_contract >= 100000, f"Expected 100000 but only received {money_locked_in_contract}"
```

Semantically it can be imagined like this:

```python
if condition_met:
    pass
else:
    print(error_message)
    <fails contract>
```

> Informative error messages will save you plenty of time when executing and debugging
> your off-chain transactions.