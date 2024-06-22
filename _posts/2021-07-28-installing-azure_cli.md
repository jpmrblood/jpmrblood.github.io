---
title: Installing Azure CLI
tags:
  - ubuntu
  - aks
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Azure CLI
---

We can install from Microsoft's repo or install from PIP. However, when using system's PIP, it would get:

```bash
/usr/lib/python3/dist-packages/requests/__init__.py:89: RequestsDependencyWarning: urllib3 (1.26.6) or chardet (3.0.4) doesn't match a supported version!
  warnings.warn("urllib3 ({}) or chardet ({}) doesn't match a supported "
```

The message is harmless, but annoying.
I want to install it as my own user.

Install `venv`:

```bash
sudo apt -y install python3-venv
```

Create an environment for Azure CLI:

```bash
mkdir -p ~/.local/share/python3
python3 -m venv ~/.local/share/python3/env-az
```

Enable that:

```bash
source ~/.local/share/python3/env-az/bin/activate
```

Install Azure CLI:

```bash
pip install azure-cli
```

# Notes

Everytime want to use the Azure CLI, just activate:

```bash
source ~/.local/share/python3/env-az/bin/activate
```

add to `~/.bashrc` to have it permanent.

I'm using `~/.local/share` path to have it FHS compliant.