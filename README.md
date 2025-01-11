# fixed-exp


[![crates.io](https://img.shields.io/crates/v/fixed-exp)](https://crates.io/crates/fixed-exp)
[![docs.rs](https://docs.rs/fixed-exp/badge.svg)](https://docs.rs/fixed-exp/)

Exponentiation for fixed-point numbers.
This is a manual fork of the [original repository](https://github.com/toasteater/fixed-exp), which has vanished.

```toml
[dependencies]
fixed-exp = { git = "https://github.com/spookyvision/fixed-exp" }
```

## Changes to the original
- changed `std::cmp` imports to `core::cmp` to facilitate `no_std` compatibility
- `fixed` broke semver by adding `is_zero()`. Removed from this crate.
- removed crash by added special handling to `powf` where the integer part of the exponent is zero
- rewrote `powi` (which is used by `powf`) to not use a greedy accumulator that would 
very quickly overflow (e.g. for `2**9` in `I16F16`). The new algorithm is slower, so
the old behavior is preserved under the `fast-powi` feature flag.

## Usage

This crate provides `powi` and `powf` for most `fixed` number types through the `FixedPowI` and `FixedPowF` extension traits:

```rust
use fixed::types::I32F32;
use fixed_exp::{FixedPowI, FixedPowF};

let x = I32F32::from_num(4.0);
assert_eq!(I32F32::from_num(1024.0), x.powi(5));
assert_eq!(I32F32::from_num(8.0), x.powf(I32F32::from_num(1.5)));
```

## License

Licensed under either of Apache License, Version 2.0 or MIT license at your option.

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in this crate by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.