<div align="center">
<img  src="https://raw.githubusercontent.com/OpShin/opshin-book/main/opshin-book.png" width="240" />
<h1 style="text-align: center;">The OpShin Book</h1>
<h3 style="text-align: center;">Introduction to the OpShin programming languages</h3>
</div>

> Disclaimer: This book is still WIP, so expect incomplete or missing information.

[Opshin](https://opshin.opshin.dev/opshin/) is a pythonic language for writing smart contracts on the [Cardano](https://cardano.org/) blockchain.
The goal of Opshin is to reduce the barrier of entry in Smart Contract development on Cardano.
Opshin is a strict subset of [Python](https://python.org/), this is means anyone who knows Python can get up to speed on Opshin pretty quickly.

## What is a Smart Contract on Cardano?

On Cardano, funds are stored at addresses. An address can be controlled by a cryptographic key, the secret for unlocking it being only known
to a specific user.
The alternative is a _Smart Contract_ - this is some encoded logic that controls unlocking funds from the address.
In particular, this logic just _validates_ that funds may be unlocked from the address based on how they are spent. For this reason,
this kind of smart contract is often referred to as _Spending Validator_.

Other contracts also exist i.e. to validate minting of native tokens, withdrawal of stake rewards or certification of stake pools.

## Sample Code

The following contract validates that the signature of a special user is present in a transaction.
The signature is specified by a third party and attached to an [UTxO](./eutxo_crash_course.md) sent to the contract.
The receiver can then construct a transaction where they unlock the funds from the contract.
The contract allows this when it is provided the signature of the receiver.

```python
# gift.py
from opshin.prelude import *


@dataclass()
class WithdrawDatum(PlutusData):
    pubkeyhash: bytes


def validator(datum: WithdrawDatum, redeemer: None, context: ScriptContext) -> None:
    sig_present = False
    for s in context.tx_info.signatories:
        if datum.pubkeyhash == s:
            sig_present = True
    assert sig_present, "Required signature missing"
```

## Why opshin?

- 100% valid Python. Leverage the existing tool stack for Python, syntax highlighting, linting, debugging, unit-testing, property-based testing, verification
- Intuitive. Just like Python.
- Flexible. Imperative, functional, the way you want it.
- Efficient & Secure. Static type inference ensures strict typing and optimized code.

#### Writing a Smart Contract

A short non-complete introduction in starting to write smart contracts follows.

1. Make sure you understand EUTxOs, Addresses, Validators etc on Cardano. [There is a wonderful crashcourse by @KtorZ](https://aiken-lang.org/fundamentals/eutxo). The contract will work on these concepts
2. Make sure you understand python. opshin works like python and uses python. There are tons of tutorials for python, choose what suits you best.
3. Make sure your contract is valid python and the types check out. Write simple contracts first and run them using `opshin eval` to get a feeling for how they work.
4. Make sure your contract is valid opshin code. Run `opshin compile` and look at the compiler erros for guidance along what works and doesn't work and why.
5. Dig into the [`examples`](https://github.com/OpShin/opshin/tree/main/examples) to understand common patterns. Check out the [`prelude`](https://opshin.opshin.dev/opshin/prelude.html) for understanding how the Script Context is structured and how complex datums are defined.
6. Check out the [sample repository](https://github.com/OpShin/opshin-starter-kit) to find a sample setup for developing your own contract.


In summary, a smart contract in opshin is defined by the function `validator` in your contract file.
The function validates that a specific value can be spent, minted, burned, withdrawn etc, depending
on where it is invoked/used as a credential.
If the function fails (i.e. raises an error of any kind such as a `KeyError` or `AssertionError`)
the validation is denied, and the funds can not be spent, minted, burned etc.

> There is a subtle difference here in comparison to most other Smart Contract languages.
> In opshin a validator may return anything (in particular also `False`) - as long as it does not fail, the execution is considered valid.
> This is more similar to how contracts in Solidity always pass, unless they run out of gas or hit an error.
> So make sure to `assert` what you want to ensure to hold for validation!

A simple contract called the "Gift Contract" verifies that only specific wallets can withdraw money.
They are authenticated by a signature.
If you don't understand what a pubkeyhash is and how this validates anything, check out [this gentle introduction into Cardanos EUTxO](https://aiken-lang.org/fundamentals/eutxo).
Also see the [tutorial by `pycardano`](https://pycardano.readthedocs.io/en/latest/guides/plutus.html) for explanations on what each of the parameters to the validator means
and how to build transactions with the contract.