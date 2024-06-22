---
layout: single
title: Installing NodeJS from NVM
tags:
  - fedora
  - nodejs
  - npm
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install NPM 
---

NodeJS is a bit volatile, so I reluctant to use native package manager. Install LTS version:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install 20
```