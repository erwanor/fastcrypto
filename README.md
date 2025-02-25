# fastcrypto
Common cryptographic library used in software at Mysten Labs.

This crate contains:
- Useful traits that should be implemented by concrete types that represent digital cryptographic 
material (keys). For signatures, we rely on `signature::Signature`, which may be more widely implemented.
- Concrete implementations of the following signature schemes that implement the recommended traits required for 
cryptographic agility. The following schemes are implemented (wrappers over existing popular crates):
    - ed25519 (EdDSA), backed by the [ed25519-consensus](https://github.com/penumbra-zone/ed25519-consensus) crate.
    - Secp256k1, backed by the [secp256k1](https://crates.io/crates/secp256k1/0.23.1) crate. 
    - BLS12-381, backed by the [blst](https://github.com/supranational/blst) crate.
- An asynchronous [`SignatureService`] (which lives in `lib.rs`) that is instantiated by a `Signer` object.

## Traits
- [`ToFromBytes`]: this trait aims to minimize the number of steps involved in obtaining a serializable key.
- [`EncodeDecodeBase64`]: an extension trait of `ToFromBytes` for immediate conversion to/from base64 strings.
- [`VerifyingKey`]: which denotes associated types for private key and signature material, while it includes a default 
naive implementation of batch verification (which can be overridden).
- [`SigningKey`]: associated types for public key and signature material.
- [`Authenticator`]: associated types for private key and public key material.
- [`KeyPair`]: which includes the common get priv/pub key functions and a key-pair generation function.

## Tests and Benchmarks
There exist tests for all the three schemes, which can be run by:  
```
$ cargo test
```

One can compare all currently implemented schemes for *sign, verify, verify_batch* and 
*key-generation* by running:
```
$ cargo bench
```
