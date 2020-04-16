---
title: 'crypto.pbkdf2Sync(password, salt, iterations, keylen, digest)'
linkTitle: 'pbkdf2Sync'
weight: 28
description: >

type: 'docs'
---

<!-- YAML
added: v0.9.3
changes:
  - version: REPLACEME
    pr-url: https://github.com/nodejs/node/pull/30578
    description: The `iterations` parameter is now restricted to positive
                 values. Earlier releases treated other values as one.
  - version: v6.0.0
    pr-url: https://github.com/nodejs/node/pull/4047
    description: Calling this function without passing the `digest` parameter
                 is deprecated now and will emit a warning.
  - version: v6.0.0
    pr-url: https://github.com/nodejs/node/pull/5522
    description: The default encoding for `password` if it is a string changed
                 from `binary` to `utf8`.
-->

- `password` {string|Buffer|TypedArray|DataView}
- `salt` {string|Buffer|TypedArray|DataView}
- `iterations` {number}
- `keylen` {number}
- `digest` {string}
- Returns: {Buffer}

Provides a synchronous Password-Based Key Derivation Function 2 (PBKDF2)
implementation. A selected HMAC digest algorithm specified by `digest` is
applied to derive a key of the requested byte length (`keylen`) from the
`password`, `salt` and `iterations`.

If an error occurs an `Error` will be thrown, otherwise the derived key will be
returned as a [`Buffer`][].

If `digest` is `null`, `'sha1'` will be used. This behavior is deprecated,
please specify a `digest` explicitly.

The `iterations` argument must be a number set as high as possible. The
higher the number of iterations, the more secure the derived key will be,
but will take a longer amount of time to complete.

The `salt` should be as unique as possible. It is recommended that a salt is
random and at least 16 bytes long. See [NIST SP 800-132][] for details.

```js
const crypto = require('crypto');
const key = crypto.pbkdf2Sync('secret', 'salt', 100000, 64, 'sha512');
console.log(key.toString('hex')); // '3745e48...08d59ae'
```

The `crypto.DEFAULT_ENCODING` property may be used to change the way the
`derivedKey` is returned. This property, however, is deprecated and use
should be avoided.

```js
const crypto = require('crypto');
crypto.DEFAULT_ENCODING = 'hex';
const key = crypto.pbkdf2Sync('secret', 'salt', 100000, 512, 'sha512');
console.log(key); // '3745e48...aa39b34'
```

An array of supported digest functions can be retrieved using
[`crypto.getHashes()`][].
