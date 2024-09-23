---
layout: single
title: Kustomize Update Field with Slash
tags:
  - kubernetes
  - kustomize
  - yaml
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/2024/07/.png
  overlay_image: /assets/2024/07/minikube.jpg
excerpt: How to insert slash as key
---

TL:DR; use `~1` will be rendered as `/`

This is a common ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/whitelist-source-range: 10.0.0.0/8
```

To change this on an overlay:

```yaml
- patch: |-
    - op: replace
      path: /metadata/annotations/nginx.ingress.kubernetes.io~1whitelist-source-range
      value: "192.168.0.0/16"
  target:
    kind: Ingress
    name: webdashboard-ingress
```

The `~1` in patch will be rendered as `/`