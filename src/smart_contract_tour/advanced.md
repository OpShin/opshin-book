# Advanced Topics

This section contains a few selected, advanced use cases of OpShin.

### Constant Folding

OpShin supports the command-line flag `--constant-folding` or short `--cf`.
With this flag, every expression is evaluated at compile time
by the python `eval` command.
On one hand this enables the precomputation of expensive constants in your code.
Specifically it evaluates all expressions that invoke only constants or variables that are
declared exactly once in the contract.
`print` statements are not pre-evaluated.
For example, the following expressions would be folded at compile time:

```python
0 == 1  # evaluates to False

@dataclass
class A(PlutusData):
    a: int
    b: bytes

A(0, b"") # evaluates to the object

def foo(x):
    return x

foo(0) # evaluates to 0

bar = 2
bar = 1
bar + 1  # is not evaluated at compile time
```

### Forcing three parameters

By setting the flag `--force-three-params` you can enable the contract to act
with any script purpose (i.e. minting, spending, certification and withdrawal).

When a script invoked with a minting, certificaton or withdrawal purpose,
the validator function is called such that the first parameter of the contract (the datum)
is set to `Nothing()` (a PlutusData object with no fields and constructor id 6).
Therefore, the compiler enforces a union type that includes `Nothing` as an
option when using the three parameter force flag.

An example of a script that acts as both minting and spending validator can be found
in the [`wrapped_token` example script](https://github.com/OpShin/opshin/blob/main/examples/smart_contracts/wrapped_token.py).

### Checking the integrity of objects

OpShin never checks that an object adheres to the structure that is declared for its parameters.
The simple reason is that this is costly and usually not necessary.
Most of the time an equality comparison (`==`) between PlutusData objects
is sufficient (and fast).

There are cases where you want to ensure the integrity of a datum however.
For example in an AMM setting where the pool datum will be re-used and potentially re-written
in subsequent calls.
In order to prevent malicious actors from making the datum too big to fit in subsequent transactions
or to prevent them from writing values of invalid types into the object, you may use [`check_integrity`](https://github.com/OpShin/opshin/blob/main/opshin/std/integrity.py#L7).

An example use case is found below.
This contract will fail if anything but a PlutusData object with constructor id 2
and two fields, first integer and second bytes, is passed as a datum into the contract.

> Note: Yes, this implies that without the explicit check the contract may pass with whatever
> type d is, since its field or constructor id are never explicitly accessed.

```python
from opshin.prelude import *
from opshin.std.integrity import check_integrity

@dataclass
class A(PlutusData)
    CONSTR_ID = 2
    a: int
    b: bytes

def validator(d: A, r: int, c: ScriptContext):
    check_integrity(d)
```
