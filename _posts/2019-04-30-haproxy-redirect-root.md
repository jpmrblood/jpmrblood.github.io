---
layout: single
title: HAProxy redirect root URL
tags:
  - haproxy
categories:
  - devops
  - web
  - tutorial
excerpt: "Configure HAProxy to redirect root URL requests to a specific application path using ACL rules and redirect directives."
---
How to redirect root URL to another URL

```ini
## Redirect root to somewhere URL
    acl is_root path -i /
    redirect code 301 location /real/app/path if is_root
```