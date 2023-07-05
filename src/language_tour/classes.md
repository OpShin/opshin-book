# Custom Classes

If you want to define a custom class to be used in Opshin, it must be a `dataclass` which inherits from the `PlutusData` class which can be imported from `opshin.prelude`.

```python
from opshin.prelude import *

@dataclass()
class Person(PlutusData):
    # Every person has a UTF8 encoded name
    name: bytes
    # Every person has a year of birth
    birthyear: int
```

PlutusData may contain only `bytes`, `int` or other dataclasses.

#### Constructing objects

You can construct an object by calling the classname with the variables in order defined in the class.

```python
a = Person(b"Billy", 1970)
```

> Note that you can also use `str` / `print` directly to get a very informative representation of the object
> ```python
> print(a)
> # prints "Person(name=b'Billy', birthyear=1970)"
> ```
#### `.to_cbor()`

To obtain the CBOR representation of an object, you may call its `to_cbor` method.
This will return the `bytes` representing an object in CBOR.

```python
print(a.to_cbor().hex())
# prints "d8799f4542696c6c791907b2ff"
```