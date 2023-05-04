# The `ScriptContext`

Majority of the logic of smart contracts has to do with making assertions about certain properties of the `ScriptContext`.
It contains a lot of useful information such as:

- When is the transaction?
- What will the inputs of the transactions be?
- What will the outputs of the transaction?

All of these things are contained in the ScriptContext object passed into the contract as the last argument.
This section covers the most interesting parts of the Script Context - if you want to learn more check out the full documentation at the module description:

https://opshin.opshin.dev/opshin/ledger/api_v2.html

## `ScriptContext`

The `ScriptContext` is defined as:

```python
@dataclass()
class ScriptContext(PlutusData):
    """
    Auxiliary information about the transaction and reason for invocation of the called script.
    """

    tx_info: TxInfo
    purpose: ScriptPurpose

```

The most important field in the `ScriptContext` is the `tx_info` field which is of type `TxInfo`.

## `TxInfo`

```python
@dataclass()
class TxInfo(PlutusData):
    """
    A complex agglomeration of everything that could be of interest to the executed script, regarding the transaction
    that invoked the script
    """

    # The input UTXOs of the transaction.
    inputs: List[TxInInfo]
    # The reference UTXOs of the transaction.
    reference_inputs: List[TxInInfo]
    # The output UTXOs created by the transaction.
    outputs: List[TxOut]
    # Transaction fee to be payed for the transaction.
    fee: Value
    # The value minted in the transaction.
    mint: Value
    dcert: List[DCert]
    wdrl: Dict[StakingCredential, int]
    valid_range: POSIXTimeRange
    # The signatures for the transaction.
    signatories: List[PubKeyHash]
    redeemers: Dict[ScriptPurpose, Redeemer]
    data: Dict[DatumHash, Datum]
    # The ID of the transaction.
    id: TxId
```

## `TxInInfo`

This type contains data about a UTXO being used as a transaction input.

```python
@dataclass()
class TxInInfo(PlutusData):
    """
    The plutus representation of an transaction output, that is consumed by the transaction.
    """

    out_ref: TxOutRef
    resolved: TxOut
```

## `TxOut`

This type contains data about a UTXO.

```python
@dataclass()
class TxOut(PlutusData):
    """
    The plutus representation of an transaction output, consisting of
    - address: address owning this output
    - value: tokens associated with this output
    - datum: datum associated with this output
    - reference_script: reference script associated with this output
    """

    address: Address
    value: Value
    datum: OutputDatum
    reference_script: Union[NoScriptHash, SomeScriptHash]
```
