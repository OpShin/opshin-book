# Validator Scripts

Congrats, if you've gotten to this section you now understand the basics of Opshin.
Now we get to put those together to write smart contracts.

If you remember from the [page on the EUTXO model](../eutxo_crash_course.md).
UTXOs are like bundles of money and a datum stored on the blockchain, they are locked by a validator script which is run when someone attempts to spend that UTXO.
Of course, this is only the case for spending contracts i.e. Contract addresses that hold actual UTxOs.
Minting Scripts and other types of validators do not have a datum as parameter.

Smart contract development on Cardano centers around writing validators in a way that allows only transactions that follow the business logic of the application.
Every validator is a simple pure function that takes two to three arguments:

1. **Datum:** This is data stored alongside the UTXO on the blockchain (in the case of Spending Validators i.e. Contract Addresses).
2. **Redeemer:** This data included in the transaction that attempts to spend the UTXO.
3. **Script Context:** This object stores information about the transaction attempting to spend the UTXO.

>**Note:**
> Datum and Redeemer are entirely controlled by the user and may not be trusted, but the Script Context may be trusted as it is assembled by the node.
> Consequently, the Datum and the Redeemer can be of any type
> depending on the contract, but the Script Context is always of type `ScriptContext`.

A validator either does not fail or fails (i.e. through the use of `assert` or an out-of-index array access).
If it does not fail then, independent of the returned value, the contract execution is counted as a success
and the contract validates the transaction.

> **Note:** Other Smart Contract languages return a boolean value that determines the success of the transaction.
> If you return `False` in OpShin, the contract will succeed in any case.



## Example Validator - Gift Contract

In this simple example we'll write a gift contract that will allow a user create a gift UTXO that can be spent by:

- The creator cancelling the gift and spending the UTXO. (1)
- The recipient claiming the gift and spending the UTXO. (2)

```python
# gift.py

# The Opshin prelude contains a lot useful types and functions 
from opshin.prelude import *


# Custom Datum
@dataclass()
class GiftDatum(PlutusData):
    # The public key hash of the gift creator.
    # Used for cancelling the gift and refunding the creator (1).
    creator_pubkeyhash: bytes

    # The public key hash of the gift recipient.
    # Used by the recipient for collecting the gift (2).
    recipient_pubkeyhash: bytes


def validator(context: ScriptContext) -> None:
    datum: GiftDatum = own_datum_unsafe(context)
    # Check that we are indeed spending a UTxO
    assert isinstance(context.purpose, Spending), "Wrong type of script invocation"

    # Confirm the creator signed the transaction in scenario (1).
    creator_is_cancelling_gift = datum.creator_pubkeyhash in context.tx_info.signatories

    # Confirm the recipient signed the transaction in scenario (2).
    recipient_is_collecting_gift = datum.recipient_pubkeyhash in context.tx_info.signatories

    assert creator_is_cancelling_gift or recipient_is_collecting_gift, "Required signature missing"
```

Two notes:
- to access the datum that was attached to the spent output, the easiest way is to use `own_datum_unsafe` which is provided to you in the prelude
- we don't access the redeemer here, because we don't need it. It would be available as `context.redeemer`

This might be a bit to take in, especially the logic for checking the signatures.
The most important part is that you see the parameters and the return type resp. the `assert` statements actually controlling the validation.
In the next chapter we'll do a deep dive into the single most important object in Cardano smart contracts, the `ScriptContext`.
