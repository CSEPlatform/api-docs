# CSE-Wallet

A lightweight wallet implementation. At the moment it supports key creation and conversion between various formats.

Motivations are:
- be lightweight
- work in a browser
- support import/export between various wallet formats
- support BIP32 HD keys

Features not supported:
- signing transactions
- managing storage (neither in node.js or the browser)

## Wallet API

Constructors:

* `generate([icap])` - create an instance based on a new random key (setting `icap` to true will generate an address suitable for the `ICAP Direct mode`)
* `generateVanityAddress(pattern)` - create an instance where the address is valid against the supplied pattern (**this will be very slow**)
* `fromPrivateKey(input)` - create an instance based on a raw private key
* `fromExtendedPrivateKey(input)` - create an instance based on a BIP32 extended private key (xprv)
* `fromPublicKey(input, [nonStrict])` - create an instance based on a public key (certain methods will not be available)
* `fromExtendedPublicKey(input)` - create an instance based on a BIP32 extended public key (xpub)
* `fromV1(input, password)` - import a wallet (Version 1 of the CSE wallet format)
* `fromV3(input, password, [nonStrict])` - import a wallet (Version 3 of the CSE wallet format). Set `nonStrict` true to accept files with mixed-caps.
* `fromCSESale(input, password)` - import an CSE Pre Sale wallet

For the V1, V3 and EthSale formats the input is a JSON serialized string. All these formats require a password.

Note: `fromPublicKey()` only accepts uncompressed CSE-style public keys, unless the `nonStrict` flag is set to true.

Instance methods:

* `getPrivateKey()` - return the private key
* `getPublicKey()` - return the public key
* `getAddress()` - return the address
* `getV3Filename([timestamp])` - return the suggested filename for V3 keystores
* `toV3(password, [options])` - return the wallet as a JSON string (Version 3 of the CSE Wallet format)

All of the above instance methods return a Buffer or JSON. Use the `String` suffixed versions for a string output, such as `getPrivateKeyString()`.

Note: `getPublicKey()` only returns uncompressed CSE-style public keys.

## HD Wallet API

To use BIP32 HD wallets, first include the `hdkey` submodule:

`var hdkey = require('cse-wallet/hdkey')`

Constructors:

* `fromMasterSeed(seed)` - create an instance based on a seed
* `fromExtendedKey(key)` - create an instance based on a BIP32 extended private or public key

For the seed  we suggest to use [bip39](https://npmjs.org/package/bip39) to create one from a BIP39 mnemonic.

Instance methods:

* `privateExtendedKey()` - return a BIP32 extended private key (xprv)
* `publicExtendedKey()` - return a BIP32 extended public key (xpub)
* `derivePath(path)` - derive a node based on a path (e.g.  m/44'/0'/0/1)
* `deriveChild(index)` - derive a node based on a child index
* `getWallet()` - return a `Wallet` instance as seen above

## Provider Engine

The Wallet can be easily plugged into [provider-engine](https://github.com/metamask/provider-engine) to provide signing:

```js
const WalletSubprovider = require('cse-wallet/provider-engine')

<engine>.addProvider(new WalletSubprovider(<wallet instance>))
```

Note it only supports the basic wallet. With a HD Wallet, call `getWallet()` first.

### Remarks about `toV3`

The `options` is an optional object hash, where all the serialization parameters can be fine tuned:
- uuid - UUID. One is randomly generated.
- salt - Random salt for the `kdf`. Size must match the requirements of the KDF (key derivation function). Random number generated via `crypto.getRandomBytes` if nothing is supplied.
- iv - Initialization vector for the `cipher`. Size must match the requirements of the cipher. Random number generated via `crypto.getRandomBytes` if nothing is supplied.
- kdf - The key derivation function, see below.
- dklen - Derived key length. For certain `cipher` settings, this must match the block sizes of those.
- cipher - The cipher to use. Names must match those of supported by `OpenSSL`, e.g. `aes-128-ctr` or `aes-128-cbc`.

Depending on the `kdf` selected, the following options are available too.

For `pbkdf2`:
- `c` - Number of iterations. Defaults to 262144.
- `prf` - The only supported (and default) value is `hmac-sha256`. So no point changing it.

For `scrypt`:
- `n` - Iteration count. Defaults to 262144.
- `r` - Block size for the underlying hash. Defaults to 8.
- `p` - Parallelization factor. Defaults to 1.

## License

MIT License

Copyright (C) 2016 CSE SG LTD
