---
layout: single
title: Install Slack in Fedora
tags:
  - fedora
  - slack
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/2024/06/tpm2.png
  overlay_image: /assets/2024/06/tpm2.png
excerpt: How to install Slack  
---
# Import Key

```bash
wget https://slack.com/gpg/slack_pubkey_20230710.gpg
sudo rpm --import slack_pubkey_20230710.gpg
```

# Download Slack Beta for Linux

Download RPM version in https://slack.com/downloads/instructions/linux?ddl=1&build=rpm
