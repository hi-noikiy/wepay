---
title: 'crypto.createHmac(algorithm, key[, options])'
linkTitle: 'createHmac'
weight: 12
description: >

type: 'docs'
---

<!-- YAML
added: v0.1.94
changes:
  - version: v11.6.0
    pr-url: https://github.com/nodejs/node/pull/24234
    description: The `key` argument can now be a `KeyObject`.
-->

- `algorithm` {string}
- `key` {string | Buffer | TypedArray | DataView | KeyObject}
- `options` {Object} [`stream.transform` options][]
- Returns: {Hmac}

Creates and returns an `Hmac` object that uses the given `algorithm` and `key`.
Optional `options` argument controls stream behavior.

The `algorithm` is dependent on the available algorithms supported by the
version of OpenSSL on the platform. Examples are `'sha256'`, `'sha512'`, etc.
On recent releases of OpenSSL, `openssl list -digest-algorithms`
(`openssl list-message-digest-algorithms` for older versions of OpenSSL) will
display the available digest algorithms.

The `key` is the HMAC key used to generate the cryptographic HMAC hash. If it is
a [`KeyObject`][], its type must be `secret`.

Example: generating the sha256 HMAC of a file

```js
const filename = process.argv[2];
const crypto = require('crypto');
const fs = require('fs');

const hmac = crypto.createHmac('sha256', 'a secret');

const input = fs.createReadStream(filename);
input.on('readable', () => {
  // Only one element is going to be produced by the
  // hash stream.
  const data = input.read();
  if (data) hmac.update(data);
  else {
    console.log(`${hmac.digest('hex')} ${filename}`);
  }
});
```
