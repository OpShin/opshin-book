# Getting Started with Opshin

If you want to learn more about OpShin you can find additional resources at these points.

#### OpShin Pioneer Program

Check out the [opshin-pioneer-program](
https://github.com/OpShin/opshin-pioneer-program) for a host of educational example contracts, test cases and off-chain code.

#### Example repository

Check out the [opshin-starter-kit](
https://github.com/OpShin/opshin-starter-kit) repository for a quick start in setting up a development environment
and compiling some sample contracts yourself.


You can replace the contracts in your local copy of the repository with code from the
`examples` section here to start exploring different contracts.

#### Awesome Opshin

The "Awesome Opshin" repository contains a host of DApps, Tutorials and other resources written in OpShin or suitable for learning it.
It can be found in the [awesome-opshin](https://github.com/OpShin/awesome-opshin) repository.

#### Developer Community and Questions

This repository contains a discussions page.
Feel free to open up a new discussion with questions regarding development using opshin and using certain features.
Others may be able to help you and will also benefit from the previously shared questions.

Check out the community [here](https://github.com/OpShin/opshin/discussions)

You can also chat with other developers [in the welcoming discord
community](https://discord.gg/umR3A2g4uw) of OpShin

> Help us improve OpShin by [participating in this survey!](https://forms.gle/KdzYYeeiWCwMTHsX9)

#### A short guide on Writing a Smart Contract

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
If you don't understand what a pubkeyhash is and how this validates anything, check out [this gentle introduction into Cardanos EUTxO](./eutxo_crash_course.md).
Also see the [tutorial by `pycardano`](https://pycardano.readthedocs.io/en/latest/guides/plutus.html) for explanations on what each of the parameters to the validator means and how to build transactions with the contract.

Minting policies expect only a redeemer and script context as argument.
Check out the [Architecture guide](https://github.com/OpShin/opshin/blob/main/ARCHITECTURE.md#minting-policy---spending-validator-double-function)
for details on how to write double functioning contracts.
The [`examples`](https://github.com/OpShin/opshin/blob/main/examples) folder contains more examples.
Also check out the [opshin-pioneer-program](
https://github.com/OpShin/opshin-pioneer-program)
 and [opshin-starter-kit](
https://github.com/OpShin/opshin-starter-kit) repo.

#### The small print

_Not every valid python program is a valid smart contract_.
Not all language features of python will or can be supported.
The reasons are mainly of practical nature (i.e. we can't infer types when functions like `eval` are allowed).
Specifically, only a pure subset of python is allowed.
Further, only immutable objects may be generated.

For your program to be accepted, make sure to only make use of language constructs supported by the compiler.
You will be notified of which constructs are not supported when trying to compile.

You can also make use of the built-in linting command and check it for example with the following command:

```bash
opshin lint spending examples/smart_contracts/assert_sum.py
```

#### Name

> Eopsin (Korean: 업신; Hanja: 業神) is the goddess of the storage and wealth in Korean mythology and shamanism. 
> [...] Eopsin was believed to be a pitch-black snake that had ears. [[1]](https://en.wikipedia.org/wiki/Eopsin)

Since this project tries to merge Python (a large serpent) and Pluto/Plutus (Greek wealth gods), the name appears fitting.
The name e_opsin is pronounced _op-shin_.
e

