# futures-lite

[![Build](https://github.com/stjepang/futures-lite/workflows/Build%20and%20test/badge.svg)](
https://github.com/stjepang/futures-lite/actions)
[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](
https://github.com/stjepang/futures-lite)
[![Cargo](https://img.shields.io/crates/v/futures-lite.svg)](
https://crates.io/crates/futures-lite)
[![Documentation](https://docs.rs/futures-lite/badge.svg)](
https://docs.rs/futures-lite)

A lightweight async prelude.

This crate is a subset of [futures] that compiles an order of magnitude faster, fixes minor
warts in its API, fills in some obvious gaps, and removes all unsafe code from it.

In short, this crate aims to be more enjoyable than [futures] but still fully compatible with
it.

[futures]: https://docs.rs/futures

## Examples

Connect to a HTTP website, make a GET request, and pipe the response to the standard output:

```rust
use async_net::TcpStream;
use blocking::{block_on, Unblock};
use futures_lite::*;

fn main() -> io::Result<()> {
    block_on(async {
        let mut stream = TcpStream::connect("example.com:80").await?;
        let req = b"GET / HTTP/1.1\r\nHost: example.com\r\nConnection: close\r\n\r\n";
        stream.write_all(req).await?;

        let mut stdout = Unblock::new(std::io::stdout());
        io::copy(&stream, &mut stdout).await?;
        Ok(())
    })
}
```

## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

#### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
