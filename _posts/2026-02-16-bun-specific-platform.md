---
layout: single
title: Run Electron on Wayland
tags:
  - bun
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Bun Specifying Platforms
---
Let me install packages only on a specific platform. Used to be Bun installing all platforms. Don't know if it's not true anymore. But I have 181 packages vs 183 packages. That's a start ;-)

```bash
bun i --cpu=x64 --os=linux
```

CPU:
> Valid values are: *, any, arm, arm64, ia32, mips, mipsel, ppc, ppc64, s390, s390x, x32, x64. Use !name to negate.

OS:
> Valid values are: *, any, aix, darwin, freebsd, linux, openbsd, sunos, win32, android. Use !name to negate.