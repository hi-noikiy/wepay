---
title: 'crypto.createVerify(algorithm[, options])'
linkTitle: 'createVerify'
weight: 18
description: >

type: 'docs'
---

<!-- YAML
added: v0.1.92
-->

- `algorithm` {string}
- `options` {Object} [`stream.Writable` options][]
- Returns: {Verify}

Creates and returns a `Verify` object that uses the given algorithm.
Use [`crypto.getHashes()`][] to obtain an array of names of the available
signing algorithms. Optional `options` argument controls the
`stream.Writable` behavior.

In some cases, a `Verify` instance can be created using the name of a signature
algorithm, such as `'RSA-SHA256'`, instead of a digest algorithm. This will use
the corresponding digest algorithm. This does not work for all signature
algorithms, such as `'ecdsa-with-SHA256'`, so it is best to always use digest
algorithm names.
