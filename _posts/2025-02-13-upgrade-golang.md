---
layout: single
title: Upgrading Golang to Newer Version
tags:
  - golang
  - upgrade
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/02/blog-with-astro.png
  overlay_image: /assets/2025/02/blog-with-astro.png
excerpt: Upgrading Golang to newer version
---

Recently Go has released new version 1.24.0. It contains an exciting upgrade for me: support for FIPS-based encryptions.

# Upgrade

Let's install:

```bash
go install golang.org/dl/go1.24.0@latest
go1.24.0 download
```

By default, Go will also add the new binary to path, but I don't want to. I have put my own directory in `${HOME}.local/bin`.

```bash
export NEW_GOBIN=`which go1.24.0`
export GOBIN=${HOME}.local/bin/go
rm -f ${HOME}.local/bin/go
ln -s $NEW_GOBIN $GOBIN
```

# Removing

If you don't want the old Go, just list the folder in your `$GOMODCACHE/golang.org`, just delete the `toolchainxxx` with the old version number.

# Upgrading Project

If you have a Go project, on the root project run:

```bash
go get -u ./...
go mod tidy
```

And commit the changes to GIT.