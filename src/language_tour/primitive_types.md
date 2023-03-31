# Primitive Types

<!-- >**Note:** This is covers -->

Opshin has 4 primitive types

- `int`
- `bool`
- `bytes`
- `str`

## `int`

This is the only numeric type in Opshin.
Opshin's `int` type works just the `int` type in Python.

```python
# Opshin supports typical integer literals:
my_decimal = 17;
my_binary  = 0b10001;
my_hex     = 0x11;
my_octal   = 0o121; ...
```

## `bool`

The `bool` type has two possible values: `True` or `False`.

```python
booly = False
```

## `bytes`

The `bytes` type in Opshin represents an array/string of bytes.
It's usually called `ByteArray` in other Cardano smart contract languages.

```python
my_bytes = b"ooh a bytestring"
```

This type is usually used to represent hashes or CBOR.
Note that bytestrings per default generate the bytestring for the ASCII character input.
If you have a bytestring `0xaf2e221a` represented in hexadecimal format, you can write it like this as a literal in opshin.

```python
hashy = b"\xaf\x2e\x22\x1a"
```

## `str`

The `str` type in Opshin is just like normal strings in Python.

```python
stringy = "hello world"

not_so_secret_message = "..."
```
