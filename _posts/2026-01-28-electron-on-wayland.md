---
layout: single
title: Run Electron on Wayland
tags:
  - wayland
  - electron
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to run Electron-based app without crashing on Wayland
---
May be works, may be not. But it sure make my laptop stable.

```bash
${ELECTRON_APP}$ --enable-features=WaylandWindowDecorations,AllowQt --ozone-platform=wayland --gtk-version=4
```

Example Google Gravity:

```bash
antigravity . --enable-features=WaylandWindowDecorations,AllowQt --ozone-platform=wayland --gtk-version=4
```
