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

> PlutusData may contain only `bytes`, `int`, dicts, lists or other dataclasses.

Note that `str` and `None` are not valid field types of PlutusData.

#### Constructing objects

You can construct an object by calling the classname with the variables in order defined in the class.

```python
a = Person(b"Billy", 1970)
```

#### Attribute access

All named attributes defined in the class body are accessible
by `object.attribute`. For example, to access the name of a person we would run

```python
print(a.name)  # prints b"Billy"
```

#### Union types

It may happen that you allow more than a single type of data for your application (think of a Smart Contract that allows different operations on it).
In this case, you may define a `Union[A, B, ...]` type.
This expresses that a variable may be of either of the classes inside the square brackets.

```python
@dataclass()
class Plant(PlutusData):
    CONSTR_ID = 1
    # Plants have no further properties

@dataclass()
class Animal(PlutusData):
    CONSTR_ID = 2
    # Animals have a name too!
    name: bytes
    # They also have an owner, which is another dataclass
    owner: Person


# Note all of these classes have distinct CONSTR_ID values
CityDweller = Union[Animal, Plant, Person]


# Both assignments are fine, because a is annotated
# to be of the Union type and can be of either class
c: CityDweller = Plant()
c = Animal(b"jackie", a)
```


> Importantly, you need to set the `CONSTR_ID` of Classes that occur in a Union to distinct values.
> On-Chain, classes are only distinguished by their `CONSTR_ID` value.
> If omitted, the `CONSTR_ID` defaults to [an almost-unique determinstic value based on the Class definition](https://github.com/cardano-foundation/CIPs/pull/608).

#### Type casts

If a variable is of an Union type we may still want to distinguish how we handle them
based on the actual type.
For this, we can use the function `isinstance`.
`isinstance(x, A)` returns `True` if value `x` is an instance of class `A` (which is not a Union type!).

```python
# this is forbidden!
# If a is a Plant or Animal, it does not have a birthyear so this operation will fail.
print(a.birthyear)

if isinstance(a, Person):
    # Here its okay
    # OpShin recognizes the isinstance check and knows that
    # a is of type Person in this branch of the condition
    print(a.birthyear)
```

We can combine isinstance calls and access shared attributes across classes.

```python
if isinstance(a, Person) or isinstance(a, Animal):
    # a is of type Union[Person, Animal] in this branch
    # Both classes have the same attribute at the same position
    # so we can access it in either case
    print(a.name)
```

You can also form the complement of type casts.

```python
a: Union[Person, Animal] = ...
if isinstance(a, Person):
    # a is of type Person in this branch
    print(a.birthyear)
else:
    # a is of type Animal in this branch
    print(a.owner)
```


> Note that you can also use `str` / `print` directly to get a very informative representation of the object
> ```python
> print(a)
> # "Person(name=b'Billy', birthyear=1970)"
> ```

#### `.to_cbor()`

To obtain the CBOR representation of an object, you may call its `to_cbor` method.
This will return the `bytes` representing an object in CBOR.

```python
print(a.to_cbor().hex())
# prints "d8799f4542696c6c791907b2ff"
```
