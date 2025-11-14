# Getting Started with Opshin

## Prequisites

This guide tries to assume as little knowledge as possible but there are certain assumptions:

- You should understand Python. Opshin is basically Python so we assume some basic knowledge of Python.

## Installation

Install Python `3.9` to `3.13`.
Then run:

```sh
python3 -m pip install opshin
```

## Compiling Opshin Code

1. Make a file called `hello_world.py` and copy:

    ```python
    # hello_world.py
    from opshin.prelude import *

    def validator(_: ScriptContext) -> None:
        print("Hello world!")
    ```

2. Run this command:

    ```sh
    opshin build hello_world.py
    ```

    This should create a `build` folder in the current directory.
    The `build` folder should look like this:

    ```sh
    build/
      └-hello_world/
        ├-mainnet.addr
        ├-script.cbor
        ├-script.plutus
        ├-script.policy_id
        └-testnet.addr
    ```

    We'll cover what all these files in the `hello_world` sub-folder mean later in the book.

