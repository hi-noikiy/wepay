---
title: 'crypto.fips'
linkTitle: 'fips'
weight: 3
description: >

type: 'docs'
---

<!-- YAML
added: v6.0.0
deprecated: v10.0.0
-->

> Stability: 0 - Deprecated

Property for checking and controlling whether a FIPS compliant crypto provider
is currently in use. Setting to true requires a FIPS build of Node.js.

This property is deprecated. Please use `crypto.setFips()` and
`crypto.getFips()` instead.
