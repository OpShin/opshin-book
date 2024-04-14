# Common Issues when interacting with OpShin

### `RecursionError: maximum recursion depth exceeded`

This may happen when your contract is too large. Try setting the flag `--recursion-limit` to something high
like 2000. If it does not go away even with values above i.e. 10000, please [open an issue](https://github.com/OpShin/opshin/issues/new/choose).