## Note:

This is only a forked package from dart_bip32_bip44 with changes thatmakes it work normally with Dart projects, applications or packages. and not only for Flutter's.

# BIP32-BIP44

[![pub package](https://img.shields.io/pub/v/dart_bip32_bip44.svg)](https://pub.dartlang.org/packages/dart_bip32_bip44)

An implementation of the [BIP32 spec] for Hierarchical Deterministic Bitcoin
addresses. No [superimposing wallet structure] has been defined.
Also supports BIP44 paths such as `m/44'/60'/0'/0/0` for Ethereum/Smart Chain

Forked from abandoned repo and updated to work with latest dependencies and null-safety because I need this library for a project

## Examples

You can use this library in two ways; one with a serialized public or private
HD key or with a hex encoded seed.

Look at the tests to see more elaborate uses.

### With a seed - BIP44 Ethereum web3dart example

```
  Chain chain = Chain.seed(hex.encode(utf8.encode("some seed")));
  ExtendedPrivateKey key = chain.forPath("m/44'/60'/0'/0/0");
  Credentials credentials = EthPrivateKey.fromHex(privateKey.privateKeyHex()); //web3dart
  var address = await credentials.extractAddress(); //web3dart
```

The `key` has a field called `key` which contains a `BigInt`. This is the actual
key. For your convenience, you can get it in hex format by calling privateKeyHex().
You can get a seed from a recovery phrase/mnemonic using bip39.

```
String seed = bip39.mnemonicToSeedHex("word word word word word word word word word word word word");
Chain chain = Chain.seed(seed);  
...
```

### Importing a HD private key

```
  Chain chain = Chain.import("xprv9s21ZrQH143K3QTDL4LXw2F7HEK3wJUD2nW2nRk4stbPy6cq3jPPqjiChkVvvNKmPGJxWUtg6LnF5kejMRNNU3TGtRBeJgk33yuGBxrMPHi");
  ExtendedPrivateKey childKey = chain.forPath("m/0/100");
```

### Importing a HD public key

```
  Chain chain = Chain.import("xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8");
  ExtendedPublic childKey = chain.forPath("M/0/100");
```

The `key` has a field called `q` which contains a [`ECPoint`]. This is the actual
key.

Please note that trying to generate a private key from a public key will throw
an exception.


## Exceptions

There is a tiny chance a child key derivation fails. Please catch the
appropriate exceptions in your code.

These exceptions are:
- `KeyZero`
- `KeyBiggerThanOrder`
- `KeyInfinite`

## Installing

Add it to your `pubspec.yaml`:

```
dependencies:
  dart_bip32_bip44: ^0.2.0
```

## Licence overview

All files in this repository fall under the license specified in 
[COPYING](COPYING). The project is licensed as [AGPL with a lesser 
clause](https://www.gnu.org/licenses/agpl-3.0.en.html). It may be used within a 
proprietary project, but the core library and any changes to it must be 
published online. Source code for this library must always remain free for 
everybody to access.

## Thanks

Without the guiding code of [go-bip32] and [money-tree] projects this library would have been a significantly bigger struggle.


[BIP32 spec]: https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki
[superimposing wallet structure]: https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#specification-wallet-structure
[go-bip32]: https://github.com/tyler-smith/go-bip32/
[money-tree]: https://github.com/GemHQ/money-tree/
[`ECPoint`]: https://pub.dartlang.org/documentation/pointycastle/1.0.0-rc3/pointycastle.api.ecc/ECPoint-class.html
