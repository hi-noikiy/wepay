---
title: 'crypto.createECDH(curveName)'
linkTitle: 'createECDH'
weight: 10
description: >

type: 'docs'
---

<!-- YAML
added: v0.11.14
-->

- `curveName` {string}
- Returns: {ECDH}

Creates an Elliptic Curve Diffie-Hellman (`ECDH`) key exchange object using a
predefined curve specified by the `curveName` string. Use
[`crypto.getCurves()`][] to obtain a list of available curve names. On recent
OpenSSL releases, `openssl ecparam -list_curves` will also display the name
and description of each available elliptic curve.
