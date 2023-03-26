# Custom Classes

If you want to define a custom class to be used in Opshin, it must be a `dataclass` which inherits from the `PlutusData` class which can be imported from `opshin.prelude`.

```python
from opshin.prelude import *

@dataclass()
class Person(PlutusData):
    # Name
    name: bytes
```
