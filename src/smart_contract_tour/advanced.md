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

### Multi-Purpose Contracts

Multi-Purpose contracts are special contracts that can act for different purposes, for example as Spending verification and as Minting verification contract. The benefit is that the contract's hash stays the same for the different purposes, allowing to derive the minting policy and the contract spending address across purposes. An example of such a contract is the ["Wrapped Token" contract](https://github.com/OpShin/opshin/blob/main/examples/smart_contracts/wrapped_token.py), which allows minting a specific token if funds are deposited at its spending address.

In order to allow Multi-Purpose contracts, you need to follow these steps:
1) Add the flag `--force-three-params` to the build command that you use, e.g. `opshin build any dual_use_contract.py --force-three-params`
2) Make sure that your contract accepts three parameters: datum, redeemer and script context
   - The datum has to be of type `Union[..., Nothing]`. When the contract is invoked with minting, certification or withdrawal purpose, the datum input will be an object of type [`Nothing()`](https://github.com/OpShin/opshin/blob/main/opshin/prelude.py#L5) and *not* the datum present at the spending script.
   - The redeemer has to be a builtin (`int`, `bytes`, etc) or a PlutusDatum with non-0 constructor id.

You need to explicitly add your contract for each minting, spending etc invocation in your final transaction. The contract will be called several times during the transaction, with different parameters in Datum, Redeemer and Purpose, depending on your configuration. 

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
