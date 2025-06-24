---
layout: single
title: Generate 32 Random Characters
tags:
  - openssl
  - pwgen
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Generate Random Characters
---
Generate 32 random characters

# OpenSSL

```bash
openssl rand -hex 32
```

# pwgen

```bash
pwgen 32 1
```

# /dev/urandom

```bash
hexdump -vn32 -e'4/8 "%08X" 1 "\n"' /dev/urandom
```

- `-v` to print all data (by default hexdump replaces repetition by *).
- `-n16` to consume 16 bytes of input (32 hex digits = 16 bytes).
- `4/8 "%08X"` to iterate four times, consume 8 bytes per iteration and print the corresponding 64 bits value as 16 hex digits, with leading zeros, if needed.
- `1 "\n"` to end with a single newline.

src: [https://stackoverflow.com/a/34329057](https://stackoverflow.com/a/34329057)

# `tr`

```
tr -dc 'A-F0-9' < /dev/urandom | dd bs=1 count=32 2>/dev/null
``