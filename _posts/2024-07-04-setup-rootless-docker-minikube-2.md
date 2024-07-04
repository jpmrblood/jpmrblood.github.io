---
layout: single
title: My Kubernetes Recipee with Minikube
tags:
  - fedora
  - minikube
  - kvm
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/2024/07/.png
  overlay_image: /assets/2024/07/minikube.jpg
excerpt: How to install Kubernetes
---
This is a continuation from [previous post](//notes/2024/06/23/fedora-minikube-kvm.html)

# Running The Command

I'm on the AMD Zen machine, can allocate more CPUs.

```bash
minikube start --driver=kvm2 --memory=12000 --cpus=4
```

Let's set the Docker registry to minikube's 

```bash
eval $(minikube docker-env)
```

Now we can build our products straight to Minikube!

```bash
docker build -t myapp/simulator:v0.0.1 .
```