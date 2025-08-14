---
layout: single
title: Install/Upgrade Golang Locally
tags:
  - golang
  - install
  - upgrade
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/02/blog-with-astro.png
  overlay_image: /assets/2025/02/blog-with-astro.png
excerpt: Installing/Upgrading Golang to newer version
---
The point is the setup.

# SETUP THIS

Let's setup in the `.bashrc`

```bash
export GOROOT=$HOME/.local/share/go
export GOPATH=$HOME/.local/lib/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

`$GOROOT` is your Golang installation. `$GOPATH` is your modules. Just figured this one.

# INSTALL

At the moment the latest version of Go is 1.25.0.

```bash

 wget https://go.dev/dl/go1.25.0.linux-amd64.tar.gz
 rm -rf $GOROOT
 tar xvfz go1.25.0.linux-amd64.tar.gz -C $HOME/.local/share/
 rm -rf go1.25.0.linux-amd64.tar.gz
```

All Golang package is designed under `go` directory. No version included in the directory. So, the binary tar.gz will always be on track.