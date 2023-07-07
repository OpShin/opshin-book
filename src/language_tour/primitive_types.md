# Primitive Types

<!-- >**Note:** This is covers -->

Opshin has 5 primitive types.
These are called primitive as they are the basis of all other types
and not composed of others.

- `int`
- `bool`
- `bytes`
- `str`
- `None`

## `int`

This is the only numeric type in Opshin.
It represents integer numbers.
Opshin's `int` type works just like the `int` type in Python and can be written in different notations.
Note that all of the below examples evaluate to an integer and can be compared and added to each other!

```python
# Opshin supports typical integer literals:
my_decimal = 17  # decimal notation
my_binary  = 0b10001  # binary notation
my_hex     = 0x11  # hexadecimal notation
my_octal   = 0o121 # octal notation

# What will this print?
print(my_decimal == my_hex)
```

#### Operation on integers

OpShin offers a number of builtin operations for integers.

```
# Addition
a = 5 + 2  # returns 7

# Subtraction
a = 5 - 2  # returns 3

# Multiplication
a = 5 * 2  # returns 10

# Integer division (floored)
a = 5 // 2  # returns 2

# Power
a = 5 ** 2  # returns 25
```

> Proper Division is not supported because UPLC has not way to represent floating point numbers.
> If you want to perform operations on rational numbers, use the [`fractions` library](https://opshin.opshin.dev/opshin/std/fractions.html)

## `bool`

The `bool` type has two possible values: `True` or `False`.
Control flow (if/else, while) are usually controlled using boolean types.

```python
booly = False
```

#### Operation on Booleans

OpShin offers a number of builtin operations for booleans.

```python
# Conjunction
b = True and False  # returns False

# Disjunction
b = True or False  # returns True

# Negation
b = not True  # returns False

# Cast to integer
b = int(True)  # returns 1 (False gives 0)
```

## `str`

The `str` type in Opshin stores Strings, i.e. human-readable text.
It is mostly used for printing debug messages.

```python
stringy = "hello world"

not_so_secret_message = "..."
```

#### Operation on Strings

OpShin offers some builtin operations for strings.

```python
# Concatentation
s = "hello " + "world!"  # Returns "hello world!"

# Cast to integer
s = int("42")  # returns 42
```

#### `.encode()`

`str` are usually stored in binary format in the so-called UTF-8 encoding.
This is for example the case for native token names.
The function `encode` transforms a normal, readable string into its binary representation.

```python
"OpShin".encode()  # returns b"\x4f\x70\x53\x68\x69\x6e"
```

#### `str()`

If you want to convert anything into a string for debugging purposes, you may call the function `str` on it.

```python
str(42)  # returns "42"
```

Note that `print` also implicitly calls `str` on its input before printing it for your convenience.

```python
print(42)  # prints "42"
```

#### Format strings

More conveniently, if you want to combine strings and other values to a nicely formatted output,
use formatting strings like this:

```python
print(f"the value of a is {a}, but the value of b is {b}")
```

This will print the original string and substitute everything between `{` and `}`
with the evaluated expression.

## `bytes`

The `bytes` type in Opshin represents an array/string of bytes.
It's usually called `ByteArray` or `ByteString` in other programming languages.
You may use it to store raw binary data.

```python
my_bytes = b"ooh a bytestring"
```

This type is usually used to represent hashes or CBOR.
Note that bytestrings per default generate the bytestring for the ASCII character input.
If you have a bytestring `0xaf2e221a` represented in hexadecimal format, you can write it like this as a literal in OpShin.

```python
hashy = b"\xaf\x2e\x22\x1a"
```

You may also use the helper function `bytes.fromhex`.

```python
hashy = bytes.fromhex("af2e221a")
```

#### Operation on ByteStrings

OpShin offers operations for bytestrings.

```python
# Concatentation
s = b"hello " + b"world!"  # Returns b"hello world!"

# Index access
k = b"hello world!"[1]  # Returns the integer of the byte at position i, in this case ord("e") = 101
```

#### `.decode()`

`bytes` may represent unicode UTF-8 encoded strings.
This is for example the case for native token names.
The function `decode` transforms a byte string into a normal, readable string.

```python
b"\x4f\x70\x53\x68\x69\x6e".decode()  # returns "OpShin"
```

#### `.hex()`

`bytes` are better readable when displayed in hexadecimal notation.
Use `hex` for this.

```python
b"\x4f\x70\x53\x68\x69\x6e".hex()  # returns "4f705368696e"
```

#### `len()`

If you want to know the length of a bytestring, call `len()` on it.

```python
len(b"OpShin")  # returns 6
```

## `None`

The `None` type is exactly like the `None` type in Python.
In other cardano smart contract languages it's called **unit** and denoted by empty brackets, `()`.
It doesn't do anything and is usually used to denote the absence of a value.

```python
null_val = None
```
