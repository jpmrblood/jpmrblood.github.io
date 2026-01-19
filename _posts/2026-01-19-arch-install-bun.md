---
layout: single
title: Install Bun in Arch Linux
tags:
  - arch
  - bun
  - javascript
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Bun in Arch Linux
---
Bun has its own installer and has frequent updates. I just set it according to FHS.

Before that, make sure `unzip` installed.

```bash
sudo pacman -S unzip
```

Then install Bun: 

```bash
curl -fsSL https://bun.com/install | BUN_INSTALL="$HOME/.local/share/bun" bash

```