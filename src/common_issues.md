# Common Issues when interacting with OpShin

### `RecursionError: maximum recursion depth exceeded`

This may happen when your contract is too large. Try setting the flag `--recursion-limit` to something high
like 2000. If it does not go away even with values above i.e. 10000, please [open an issue](https://github.com/OpShin/opshin/issues/new/choose).

### The contract is too large to submit in a transaction

OpShin provides various optimizations to decrease contract size. Most of these are already activated by default but a number of [powerful optimizations can be added](https://book.opshin.dev/smart_contract_tour/advanced.html#compiler-flags):

- `-fdeduplicate` is a powerful optimization that can reduce contract size significantly. It deduplicates low level terms, but is expensive, which is why it is disabled by default.
- `-O3` enables a range of powerful optimizations (including `-fdeduplicate`) but also removes all error traces and print statements. This significantly reduces contract size but makes debugging much more difficult. 
