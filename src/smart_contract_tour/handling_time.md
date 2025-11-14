# Handling Time

In a lot of contracts, time is of the essence.
Properly handling time is essential for a lot of contracts like auctions and swaps.

The `valid_range` property in the `ScriptContext` specifies within what time the transaction can be submitted, it's used to constrain the time of the transaction.
It's of type `POSIXTimeRange`.

## `POSIXTimeRange`

The `POSIXTimeRange` type is used to specify a time range.

```python
dataclass()
class POSIXTimeRange(PlutusData):
    """
    Time range in which this transaction can be executed
    """

    # Lower bound for the execution of the transaction
    # Can be negative infinity or a int.
    lower_bound: LowerBoundPOSIXTime
    # Upper bound for the execution of the transaction
    # Can be positive infinity or a int.
    upper_bound: UpperBoundPOSIXTime
```

A `POSIXTimeRange` can be visualized like a subsection of a numberline starting from negative infinity(`-∞`) to positive infinity (`+∞`)

```sh
         ⬤-------------→⬤
-∞       -3 -2  -1   0   1   2   3       +∞
 └┴┴┴┴┴─┴─┴──┴───┴───┴───┴───┴───┴─┴─┴┴┴┴┘ 
```

### Useful Methods

Helper functions for the `POSIXTimeRange` type are stored in [`opshin.ledger.interval`](https://opshin.opshin.dev/opshin/ledger/interval.html#opshin.ledger.interval).


#### `make_range(lower_bound: int, upper_bound: int) -> POSIXTimeRange`

This takes a the lower bound as the first argument and the upper bound as the second argument makes a `POSIXTimeRange` that starts from `lower_bound` and ends at `upper_bound`.

```python
valid_range = make_range(-3, 2)
```

This can be visualized as:

```sh
     lower_bound        upper_bound
          ⬤----------------→⬤
-∞       -3 -2  -1   0   1   2   3       +∞
 └┴┴┴┴┴─┴─┴──┴───┴───┴───┴───┴───┴─┴─┴┴┴┴┘ 
```

#### `make_from(lower_bound: int) -> POSIXTimeRange`

This takes a the lower bound as the first argument and makes a `POSIXTimeRange` that starts from `lower_bound` and ends at positive infinity.

```python
valid_range = make_from(-1)
```

This can be visualized as:

```sh
            lower_bound     
                 ⬤---------------------→⬤
-∞       -3 -2  -1   0   1   2   3       +∞
 └┴┴┴┴┴─┴─┴──┴───┴───┴───┴───┴───┴─┴─┴┴┴┴┘ 
```

#### `make_to(upper_bound: int) -> POSIXTimeRange`

This takes a the upper bound as the first argument and makes a `POSIXTimeRange` that starts from negative infinity and ends at `upper_bound`.

```python
valid_range = make_to(1)
```

This can be visualized as:

```sh
                    upper_bound 
 ⬤---------------------→⬤
-∞       -3 -2  -1   0   1   2   3       +∞
 └┴┴┴┴┴─┴─┴──┴───┴───┴───┴───┴───┴─┴─┴┴┴┴┘ 
```

## Vesting Contract

To really grok how time is managed on Cardano we'll write a simple vesting contract.
