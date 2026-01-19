---
layout: single
title: Install Golang in Arch Linux
tags:
  - arch
  - golang
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Golang in Arch Linux
---
Golang doesn't have its own installer, so we can just install it with `pacman`.

```bash
sudo pacman -S go
```

I like my Golang workspace in FHS style. The default is in `$HOME/go`.

```bash
mkdir ~/.local/share/go
go env -w GOPATH=$HOME/.local/share/go
```

Add to PATH, edit `~/.bashrc`:

```bash
export PATH=$PATH:$(go env GOPATH)/bin
```
