# Changelog

# Version 3.0.0 (Not Released)

* Use [paragonie/constant_time_encoding](https://github.com/paragonie/constant_time_encoding) 
* We now default to URL-safe Base 64 encoding (RFC 4648) 
* API change: Now instead of a plain `string` scalar, you will be passing 
  instances of `HiddenString` back and forth. Should an unhandled exception
  ever occur, you will be spared the pain of data leaks via stack trace.
* Dropped support for version 1.
  * We no longer offer or use scrypt anywhere. Everything is Argon2 now.
  * `KeyFactory` no longer accepts a `$legacy` argument.

## Version 2.1.2 (2016-07-11)

* Better docblocks, added unit test to prevent regressions.

## Version 2.1.1 (2016-05-15)

* Prevent an undefined index error when calculating the root of an empty MerkleTree.

## Version 2.1.0 (2016-05-07)

* Key derivation (via `KeyFactory`) can now accept an extra argument to 
  specify the security level of the derived key.
  * Scrypt: `INTERACTIVE` or `SENSITIVE`
  * Argon2i: `INTERACTIVE`, `MODERATE`, or `SENSITIVE`
* `Password` can now accept a security level argument. We recommend
  sticking with `INTERACTIVE` for end users, but if you'd rather make
  administrative accounts cost more to attack, now you can make that
  happen within Halite.
* `MerkleTree` can now accept a personalization string for the hash 
  calculation.
* `MerkleTree` can output a specific hash length (between 16 and 64).
* Both `MerkleTree` and `Node` now lazily calculate the Merkle root 
  rather than calculating it eagerly. This results in less CPU waste.
* Cleaned up the legacy cruft in the `Key` classes. Now they only accept
  a string in their constructor.

## Version 2.0.1 (2016-04-20)

* Fixed conflict with PHP 7 string optimizations that was causing `File::decrypt()` to fail in PHP-FPM.
* Introduced a new method, `Util::safeStrcpy()`, to facilitate safe string duplication without triggering the optimizer.

## Version 2.0.0 (2016-04-04)

* Halite now requires:
  * PHP 7.0+
  * libsodium 1.0.9+
  * libsodium-php 1.0.3+
  * (You can use `Halite::isLibsodiumSetupCorrectly()` to verify the
    latter two)
* Strictly typed everywhere
* You can no longer pass a well-configured but generic `Key` object to
  most methods; you must pass the appropriate child class (i.e.
  `Symmetric\Crypto::encrypt()` expects an instance of 
  `Symmetric\Crypto\EncryptionKey`.
* Updated password hashing and key derivation to use Argon2i
* `File` now uses a keyed BLAKE2b hash instead of HMAC-SHA256.
* `Key->get()` was renamed to `Key->getRawKeyMaterial()`
* `Password` now has a `needsRehash()` method which will return `true`
  if you're using an obsolete encryption and/or hashing method.
* `Util` now has several new methods for generating BLAKE2b hashes:
  * `hash()`
  * `keyed_hash()`
  * `raw_hash()`
  * `raw_keyed_hash()`
* Removed most of the interfaces in `Contract`
