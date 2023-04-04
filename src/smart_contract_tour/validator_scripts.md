# Validator Scripts

Congrats, if you've gotten to this section you now understand the basics of Opshin.
Now we get to put those together to write smart contracts.

If you remember from the [page on the EUTXO model](../eutxo_crash_course.md).
UTXOs are like bundles of money and a datum stored on the blockchain, they are locked by a validator script which is run when someone attempts to spend that UTXO.

Smart contract development on Cardano centers around writing validators in a way that allows only transactions that follow the business logic of the application.

Every validator is a simple pure function that takes three arguments:

1. **The Datum:** This is data stored alongside the UTXO on the blockchain.
2. **The Redeemer:** This data included in the transaction that attempts to spend the UTXO.
3. **The ScriptContext:** This object stores information about the transaction attempting to spend the UTXO.

and returns a boolean that determines if the transaction is succesful.

>**Note:** The Datum and the Redeemer can be off any type,
>depending on the contract but the ScriptContext is always of type `ScriptContext` but can be set to type `Nothing` if not in use.

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


def validator(datum: CancelDatum, redeemer: None, context: ScriptContext) -> None:
    # Confirm the creator signed the transaction in scenario (1).
    creator_is_cancelling_gift = datum.creator_pubkeyhash in context.tx_info.signatories

    # Confirm the recipient signed the transaction in scenario (2).
    recipient_is_collecting_gift = datum.pubkeyhash in context.tx_info.signatories

    return creator_is_cancelling_gift or recipient_is_collecting_gift
    # assert sig_present, "Required signature missing"
```

This might be a bit to take in, especially the logic for checking the signatures.
In the next chapter we'll do a deep dive into the single most important object in Cardano smart contracts, the `ScriptContext`.
