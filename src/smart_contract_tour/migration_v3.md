# Migrating to PlutusV3

If you previously wrote contracts in OpShin using PlutusV2 (version 0.26.1 or earlier), this guide helps you migrate to PlutusV3 (version 0.27.0 and later).

### Obtaining datum and redeemer

The biggest change compared to PlutusV2 is, that all contracts (minting, spending, etc.) now expect only a single parameter, the Script Context.
The redeemer and datum are now stored inside the Script Context.

Below is an example of how to modify your contract to quickly adapt to the new format. If your contract looked like this before...
```python
def validator(datum: Listing, redeemer: ListingAction, context: ScriptContext) -> None:
    ...
```

You can adapt it like this for the new PlutusV3 format.
```python
def validator(context: ScriptContext) -> None:
    datum: Listing = own_datum_unsafe(context)
    redeemer: ListingAction = context.redeemer
    ...
```

For non-spending contracts, you may want to skip the part regarding `datum`. 

### Naming changes in the ledger API

Please inspect the [ledger API](https://opshin.opshin.dev/opshin/ledger/api_v3.html) for potential changes in the structure and names of Script Context fields.
The most common differences:

- `ScriptContext.tx_info` is now called `ScriptContext.transaction`
- `TxInfo.valid_range` is now called `TxInfo.validity_range`


### Returning None

PlutusV3 enforces that validators return `BuiltinUnit`. In OpShin, this corresponds to the Python value `None`, and is the default return value, for example if your contract does not end with a `return` statement. However, you need to make sure that the return value of the validator is annotated with `-> None`. Thus, the standard signature for any smart contract on PlutusV3 is now like this.
```python
def validator(context: ScriptContext) -> None:
```

### Script parameters

If you previously paramterized your script, you need to now explicitly specify the number of left-over parameters of your contract after applying command-line arguments. For example, to create a minting contract that accepts a single `bytes` parameter with a validator like this:
```python
# validator.py
def validator(tk_name: bytes, context: ScriptContext) -> None:
    ...
```

You need to compile it like this
```
opshin build validator.py --parameters 1
```

This ensures that you do not run into unexpected runtime errors when you mistakenly still expect datum and redeemer parameters.

If you already parameterize during building, you specify the number of left-over parameters, if any. For example for the following contract, where we bind the first two during building, we specify 1 as the number of left-over parameters.

```python
# validator2.py
def validator(owner: bytes, after: int, tk_name: bytes, context: ScriptContext) -> None:
    ...
```

```
opshin build validator2.py '{"bytes": "DEADBEEF"}' '{"int": 42}' --parameters 1
```

### Multi-purpose contracts

By default, all contracts in PlutusV3 can act as multi-purpose contracts. The flag `--force-three-params` was removed.

### New builtins

The PlutusV3 language comes with a range of new builtins, in particular for bitwise manipulation and operations on the BLS 12 381 rings, enabling the verification of zero-knowledge proofs on-chain.
You can find more details about all builtins by inspecting the [builtin library](https://opshin.opshin.dev/opshin/std/builtins.html).