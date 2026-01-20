---
layout: single
title: Install Podman Docker-compatible in Arch Linux
tags:
  - arch
  - podman
  - docker
  - crun
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Podman with Docker compatibility
---
I assume you have installed `yay`.

```bash
yay -Syu podman podman-docker docker-compose
```

Tell the system we ain't going to use Docker.

```bash
sudo touch /etc/containers/nodocker
```

Run it rootless:

```bash
systemctl enable --user podman --now
```
