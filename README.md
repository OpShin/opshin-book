# Opshin Book

> The documentation is now included with the [main opshin website](https://github.com/opshin/site). This website was replaced with a redirect.

Official documentation for the Opshin programming language.

## Running locally

To run this project, you'll need:
- Git
- [mdBook](https://rust-lang.github.io/mdBook/guide/installation.html)

1. Clone the repo:
   ```sh
   $ git clone https://github.com/Ch1n3du/opshin_book
     ...
   ```
2. Install Rust and mdbook
   ```sh
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   cargo install mdbook
   ```
3. Open the book directory in the terminal and run: 
   ```sh
   $ cd opshin_book
   $ mdbook serve
     ...
   ```
   This will start a local server hosting the site.
