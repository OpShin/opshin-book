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

### Supporters

<a href="https://github.com/inversion-dev"><img src="https://avatars.githubusercontent.com/u/127298233?s=200&v=4" width="50"></a>
<a href="https://github.com/MuesliSwapTeam/"><img  src="https://avatars.githubusercontent.com/u/91151317?v=4" width="50" /></a>
<a href="https://github.com/AadaFinance/"><img  src="https://avatars.githubusercontent.com/u/89693711?v=4" width="50" /></a>
<a href="https://github.com/kreate-community/"><img  src="https://avatars.githubusercontent.com/u/118675270?v=4" width="50" /></a>

The main sponsor of this project is [Inversion](https://inversion.dev/cardano/). Here is a word from them!

> At Inversion, we pride ourselves on our passion for life and our ability to create exceptional software solutions for our clients. Our team of experts, with over a century of cumulative experience, is dedicated to harnessing the power of the Cardano blockchain to bring innovative and scalable decentralized applications to life. We've successfully built applications for NFT management, staking and delegation, chain data monitoring, analytics, and web3 integrations, as well as countless non-blockchain systems. With a focus on security, transparency, and sustainability, our team is excited to contribute to the Cardano ecosystem, pushing the boundaries of decentralized technologies to improve lives worldwide. Trust Inversion to be your go-to partner for robust, effective, and forward-thinking solutions, whether blockchain based, traditional systems, or a mix of the two.

They have recently started a podcast, called "Africa On Chain", which you can check out here:
https://www.youtube.com/@africaonchain
