---
layout: single
title: Building Distroless image with APKO
tags:
  - apko
  - chainguard
  - distroless
  - image
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: https://raw.githubusercontent.com/distroless/.github/main/profile/distroless-logo.svg
  overlay_image: https://raw.githubusercontent.com/distroless/.github/main/profile/distroless-logo.svg
excerpt: The thing I know about building Distroless using APKO
---
Chainguard Academy provides distroless images for us to use. Most of their images are provided free. However, most of the specific versions and the FIPS versions are for paid channels.

Fortunately, they are kind enough for us to build one ourselves using their tool: `apko`.

# Install APKO

APKO can be run by using Docker or you could just install it by yourself.

```bash
go install chainguard.dev/apko@latest
```
Or, download from [Releases page](https://github.com/chainguard-dev/apko/releases).

# Using APKO

It uses a simple YAML file for configuration. For example, I am trying to build Python 3.11 image. The YAML file:

```yaml
contents:
  keyring:
    - https://packages.wolfi.dev/os/wolfi-signing.rsa.pub
  repositories:
    - https://packages.wolfi.dev/os
  packages:
    - ca-certificates-bundle
    - python-3.11

entrypoint:
  command:      /usr/bin/python

environment:
  PATH: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  SSL_CERT_FILE: /etc/ssl/certs/ca-certificates.crt

archs:
  - x86_64

```
# Build with APKO

## Using Commandline

```bash
apko build python-base.yaml python-base:edge python-base.tar
```

## Using Docker

```bash
docker run -v "$PWD":/work cgr.dev/chainguard/apko build python-base.yaml python-base:edge python-base.tar
```
# Import to Docker 

```bash
docker load < python-base.tar

```

# Next
Chainguard Academy also provides a packaging system: `Melange`. I'm interested to build and package my app using that and integrate with `apko`.

# References
- [APKO Github page](https://github.com/chainguard-dev/apko)