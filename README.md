# Not *Yet* Awesome Embedded Rust

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

This is a collection of things that are **not yet** awesome in Embedded Rust. We need your help to capture all of the things that are not yet awesome, and to start making them awesome!

[<img src="ewg-logo-blue-white-on-transparent-256x256.png" align="right" width="256">](https://github.com/rust-embedded/wg)

This project is developed and maintained by the [Resources team][team].

Are you interested in:

* Adding something to this list that is not yet awesome?
* Tell people you are working on making something on this list awesome?
* Tell people you have made something on this list awesome already?

Check out our [Contributing Guide] for how to get started.

[Contributing Guide]: CONTRIBUTING.md

And don't forget to check our [Awesome Embedded Rust][aer] list! The thing you are looking for may already exist!

[aer]: https://github.com/rust-embedded/awesome-embedded-rust

## Table of Contents

* Useful Links
    * [Contributing Guide]
    * [Item Template](#not-yet-awesome-item-template)
* Not Yet Awesome List
    * [Sharing Data With Interrupts](#sharing-data-with-interrupts)
    * [Serialization/Deserialization in `no_std`](#serializationdeserialization-in-no_std)

# The List

## Sharing Data With Interrupts

### Background

Currently, it is not convenient to share non-atomic or non-`Sync` data with an interrupt using safe Rust. Because interrupt handlers are functions that take no arguments, all data consumed by interrupts must either be global or module scoped Atomics (e.g. `AtomicBool`), local `static` variables, or global or module scoped `static` variables.

Global variables are not great in Rust, because:

* All mutable access (of non-`Sync`/`Atomic` data) must be `unsafe`
* Not all data can be initialized in a `const` context, so it's often necessary to use an `Option<T>` to delay the initialization to runtime
* Global variables aren't typically idiomatic Rust.

Frameworks like [cortex-m-rtfm] achieve this in a zero cost fashion by using a Domain Specific Language to automatically provide safe access to shared resources between tasks and interrupts, however these tools can not be used by applications not using RTFM, or by libraries such as HAL or BSP crates.

**Useful Links**

* [wg#294] - An Embedded-WG issue discussing this topic
* [bare-metal#15] - One proposed solution hiding the un-idiomatic syntax

[wg#294]: https://github.com/rust-embedded/wg/issues/294
[bare-metal#15]: https://github.com/japaric/bare-metal/pull/15
[cortex-m-rtfm]: https://github.com/japaric/cortex-m-rtfm

### Success Criteria

Ideally, we would be able to support all of the following use cases:

1. Sharing a variable between the main thread and only one interrupt handler
2. Sharing a variable between the main thread and one or more interrupt handlers
3. Moving a variable from the main thread to one interrupt handler

We should be able to serve the three use cases listed above, while:

* Using only safe Rust (at least as a user of the library)
* Add only minimal overhead, if not "zero cost"

<!-- TODO: Uncomment when there is work in progress -->
<!-- ### Work in progress -->

## Serialization/Deserialization in `no_std`

### Background

For embedded systems that send or receive data to other devices, or who read/write data to a medium such as an SD card or other Flash memory, it is useful to have automatic serialization and deserialization support to turn structured data into a binary format.

While tools like [`serde`] are widely used in Rust (and already support `no_std`), few of the the backends (like [`serde_json`]) are supported in a `no_std` environment.

[`serde`]: https://github.com/serde-rs/serde
[`serde_json`]: https://github.com/serde-rs/json
[`ssmarshal`]: https://gitlab.com/robigalia/ssmarshal
[`ujson`]: https://github.com/japaric/jnet/tree/master/ujson
[`bincode-no-std`]: https://github.com/losfair/bincode
[`cbor-no-std`]: https://crates.io/crates/cbor-no-std

### Success Criteria

We should be able to serialize and deserialize data in an automatic way, either using a frontend like [`serde`], or with a simple and convenient `serialize` and `deserialize` method. To use these, we should not need heap allocations, and it should be possible to detect when serialization and deserialization have failed.

Additionally, all common datatypes used in embedded Rust should be supported, as well as a (preferrably automatic) way to add support for custom data types. Common data types include:

* Fixed point signed/unsigned numbers like `u8..u64` or `i8..i64`
* Floating point numbers like `f32..f64`
* Booleans
* Variable length types, like `[u8]` and `&str`
* Tuples
* Enumerated types

We should also have support for different ways of serializing data, primarily:

* Self-describing schemas, such as JSON or CBOR
* Non-self-describing schemas, such as binary packed data or ProtoBufs

### Work in progress

* [`serde`] already has support for `no_std` environments, when using a feature flag
    * Serde welcomes [contributions](https://github.com/serde-rs/json/blob/master/CONTRIBUTING.md)
* [`ssmarshal`] has support for basic types, but does not support enums with more than 255 variants, or variable sized types like slices. This covers only the non-self-describing use case.
    * ssmarshal welcomes [contributions](https://gitlab.com/robigalia/ssmarshal/blob/master/CONTRIBUTING.md)
* [`ujson`] supports JSON serialization and deserialization for `no_std` environments, but is experimental (and not on crates.io), and is unlikely to be stabilized as-is
    * It is unknown if ujson welcomes contributions
* There are a number of forks of `std`-only `serde` backends, though they do not seem to have official support from the upstream libraries. These forks include:
    * [`bincode-no-std`]
    * [`cbor-no-std`]

# Not Yet Awesome Item Template

Here's an example for something that is not yet awesome:

## It's Hard to Foo without a Bar

### Background

[IETF RFC -1](#) states that you should be able to Foo with either a Bar, Bib, Bim, or Bap. However in all of [these](#) [rust](#) [crates](#), you're required to have a Bar!

### Success Criteria

I'd like to be able to Foo using **only** a Bib, Bim, or Bap.

### Work in progress

* The [foobar](https://crates.io/crates/foobar) crate has [Issue #1337](#) open to add support for Foo-ing with a Bim or Bap.
    * The `foobar` crate is on [on github](#)
    * They are looking for contributors (see [issue #1338](#))
    * They have a [Code of Conduct](CODE_OF_CONDUCT.md)

# License

The Not Yet Awesome Embedded Rust list (this project) is distributed under the terms of the Creative Commons [CC-BY-SA v4.0] license.

Copies of the license used by this project may also be found here: [CC-BY-SA v4.0 Hosted].

[CC-BY-SA v4.0]: ./LICENSE-CC-BY-SA
[CC-BY-SA v4.0 Hosted]: https://creativecommons.org/licenses/by-sa/4.0/legalcode

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you shall be licensed as above, without any additional terms or conditions.

# Code of Conduct

Contribution to this crate is organized under the terms of the [Rust Code of
Conduct][CoC], the maintainers of this crate, the [Resources team][team], promises
to intervene to uphold that code of conduct.

[CoC]: CODE_OF_CONDUCT.md
[team]: https://github.com/rust-embedded/wg#the-resources-team
