---
layout: single
title: Rootless Docker in Fedora
tags:
  - fedora
  - docker
  - rootless
  - minikube
  - kubernetes
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Rootless Docker
---
# Running The Command
As [documentation said](https://minikube.sigs.k8s.io/docs/drivers/docker/#Rootless%20Docker):

```bash
dockerd-rootless-setuptool.sh install -f
docker context use rootless
minikube start --driver=docker --container-runtime=containerd
```

# The Command Output

We might need to put this on the `.bashrc`

```bash
export DOCKER_HOST=unix:///run/user/1000/docker.sock
```

They'll give you something to work with.

