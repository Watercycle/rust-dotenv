rust-dotenv
====

**Achtung!** This is an alpha version! Expect bugs and issues all around.
Submitting pull requests and issues is highly encouraged!

Quoting [bkeepers/dotenv](https://github.com/bkeepers/dotenv):

> Storing [configuration in the environment](http://www.12factor.net/config)
> is one of the tenets of a [twelve-factor app](http://www.12factor.net/).
> Anything that is likely to change between deployment environments–such as
> resource handles for databases or credentials for external services–should
> be extracted from the code into environment variables.

This library is meant to be used on development or testing environments in
which setting environment variables is not practical. It loads environment
variables from a `.env` file, if available, and mashes those with the actual
environment variables provided by the operative system.

Usage
----

The aim of this project is to be as close as possible to a drop-in replacement
for `std::os::env`. Because of this, the API exposed by the standard library
is imitated. The methods provided by a Dotenv struct, `env`, `env_as_bytes`,
`getenv` and `getenv_as_bytes`, carry the same signatures as their standard
library counterparts.

Dotenv implements the `dotenv` static method, returning a Dotenv
struct using the contents of the file named `.env` at the path of your
application binary, if it exists. If you need finer control
about the source of the environment variables, Dotenv exposes the static
methods `from_path`, `from_file`, `from_filename`, `from_bytes` and `from_str`.

Examples
----

A `.env` file looks like this:

```sh
REDIS_ADDRESS=localhost:6379
MEANING_OF_LIFE=42
```

A sample project using Dotenv would look like this:

```rust
extern crate dotenv;

use dotenv::Dotenv;

fn main() {
    let dotenv = Dotenv::dotenv();
    for (key, value) in dotenv.env().into_iter() {
        println!("key: {}, value: {}", key, value)
    }
}
```

Dotenv also implements the `Default` trait, which returns a Dotenv struct
without any content of its own; that is, containing only the environment
variables exported by the system. This makes it easy to ignore IO failures if
the environment variable file can't be found:

```rust
let dotenv = Dotenv::from_filename("nonexisting.env").ok().unwrap_or_default();
```
