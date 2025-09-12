---
layout: single
title: Getting LDAP Epoch
tags:
  - ldap
categories:
  - ldap
  - terminal
  - php
  - tutorial
excerpt: "How to get LDAP epoch timestamps and convert dates for LDAP directory operations using PHP and command line tools."
---
Run this on the server:
```
$ echo ' <?php echo strtotime("+185 days")/86400;' | php
```
This is to have date 185 days from today in LDAP format. Useful to set LDAP expiry date.
