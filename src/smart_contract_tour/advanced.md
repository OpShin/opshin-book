# Advanced Topics

This section contains a few selected, advanced use cases of OpShin.

### Compiler Flags

OpShin provides several flags that enable, control or disable optimizations.
Concretely the following flags are available (visible when calling `opshin --help`):

| Flag | Description |
|------|-------------|
| `-fcompress-patterns` |  Enables the compression of re-occurring code patterns. Can reduce memory and CPU steps but increases the size of the compiled contract. |
|`-fiterative-unfold-patterns`| Enables iterative unfolding of patterns. Improves application of pattern optimization but is very slow. |
|`-fconstant-index-access-list` |  Replace index accesses with constant parameters with optimized constant accesses. Reduces memory and CPU steps but increases the size of the compiled contract. |
|`-fconstant-folding`, `--cf` | Enables experimental constant folding, including propagation and code execution. See below for a detailed explanation. |
|`-fremove-dead-code`|    Removes dead code and variables from the contract. |
|`-ffast-access-skip FAST_ACCESS_SKIP`| How many steps to skip for fast list index access, default None means no steps are skipped (useful if long lists are common). |
| `-fdeduplicate` | Removes duplicated parts of the AST from the lowest level program representation, the UPLC level. This can slightly increase execution cost at a significant reduction of size |
| `-fremove-traces` | Removes all error messages, print statements and other traces from the program. As OpShin usually prints extensive debug information, this significantly reduces contract size, but makes debugging much harder. |

### Optimization Levels

OpShin, similar to C-compilers, features optimization levels. These control which optimizations are applied to the generated code to make it smaller and more performant, but at the same time more difficult to debug.
The levels reach from 0 to 3, where higher numbers indicate more optimizations.
However, as opposed to C-programs, there are not yet many debugging tools that require non-optimized ode.
Thus, as a typical user, optimizing less than level 2 will not bring any debugging benefits.
Therefore, OpShin per default optimizes at level 2.
Level 3 enables some slow optimizations and in theory allows optimizations that change the semantics, i.e., the behavior, of the contract.
In practice, this will never have more of an effect than removing print statements or assertion messages.
However, these messages are quite useful in practice and thus level 3 should not be enabled unless the contract has been thoroughly tested.

The following optimizations are enabled by each optimization level (where each optimization level contains all optimizations of the lower optimizations):

- `O0`: None
- `O1`: 
    - `-fcompress-patterns`
    - `-fconstant-index-access-list`
    - `-fremove-dead-code`
- `O2`:
    - `-fconstant-folding`
    - `-ffast-access-skip 5`
- `O3`:
    - `-fiterative-unfold-patterns`
    - `-fdeduplicate`
    - `-fremove-traces`

Flags can be overridden by passing them specifically after specifying an optimization level, e.g., `opshin build ... -O2 -ffast-access-skip 2`.


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

By default in PlutusV3, all contracts can act as multi-purpose contracts.

When building a transaction that executes your smart contract,
you need to explicitly add your contract for each minting, spending etc invocation in your final transaction. The contract will be called several times during the transaction, with different parameters in Datum, Redeemer and Purpose, depending on your configuration. 

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
