---
layout: single
title: Installing Minikube to use KVM2 in Fedora
tags:
  - fedora
  - kvm
  - minikube
  - docker
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to create Minikube with KVM  
---

Enable Hypervisor in BIOS, first.

Install Docker for just in case, in other post.

Install QEMU and Libvirt:

```bash
sudo dnf install libvirt qemu-kvm qemu-system-x86-core
```

Enable additional `CGroups` ability. Too bad my AMD processor doestn't have SVE. So, secure guest is not supported.

```bash
 sudo grubby --args="intel_iommu=on systemd.unified_cgroup_hierarchy=0" --update-kernel=ALL
 sudo dracut -f
```

Add users:

```bash
sudo usermod -aG docker $USER
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

Restart. Or use `newgrp`.

# Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64 && sudo install -m0755 minikube-linux-amd64 /usr/local/bin/minikube  
minikube start --driver kvm2 --cpus=4 --memory=12000
minikube addons enable ingress
```

Add `kubectl`

```bash
alias k="minikube kubectl -- "
alias kubectl=k
```