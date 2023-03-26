# Primitive Types

<!-- >**Note:** This is covers -->

Opshin has 4 primitive types

- `int`
- `bool`
- `bytes`
- `string`

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

The `bool` type has two possible values: `true` or `false`.

```python
booly = false
```

## `bytes`

The `bytes` type in Opshin represents an array/string of bytes.
It's usually called `ByteArray` in other Cardano smart contract languages.

```python
my_bytes: bytes = b"ooh a bytestring"
```

This type is usually used to represent hashes or CBOR.

```python
hashy: bytes = b"af2e221a"
```

## `string`

The `string` type in Opshin is just like normal strings in Python.

```python
stringy: str = "hello world"

not_so_secret_message = "..."
```
