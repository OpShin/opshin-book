# Builtins

A growing list of the Python builtin functions [is being implemented in OpShin](https://github.com/OpShin/opshin/issues/44).
It will be documented here.

#### `all(x: List[bool]) -> bool`

Returns whether all booleans in a list are `True`.

#### `any(x: List[bool]) -> bool`

Returns whether any of the booleans in a list is `True`.

#### `abs(x: int) -> int`

Returns the absolute value of an integer.

#### `bytes(x: int) -> bytes`

Returns a bytestring with 0s of length `x`.

#### `bytes(x: List[int]) -> bytes`

Returns a bytestring where every byte corresponds to one integers in `x`.

#### `chr(x: int) -> str`

Returns the character for unicode code point `x`.

#### `hex(x: int) -> str`

Returns the hexadecimal representation of an integer.

#### `len(x: Union[bytes, List[Anything]]) -> int`

Returns a the length of `x` for bytes and lists.

#### `max(x: List[int]) -> int`

Returns the maximum of all integers in a list.

#### `min(x: List[int]) -> int`

Returns the minimum of all integers in a list.

#### `oct(x: int) -> str`

Returns the octal representation of an integer.

#### `pow(x: int, y: int) -> str`

Returns x<sup>y</sup>

#### `range(x: int) -> List[int]`

Returns the integers from 0 to `x-1` as a list.

#### `reversed(x: List[Anything]) -> List[Anything]`

Returns the reversal of list `x`.

#### `str(x: Anything) -> str`

Returns a stringified representation of `x`.

#### `sum(x: List[int]) -> int`

Returns the sum of all integers in a list.