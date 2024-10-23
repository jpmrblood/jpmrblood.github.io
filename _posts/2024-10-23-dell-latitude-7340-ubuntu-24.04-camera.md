---
layout: single
title: Intel IPU6 Camera on Kubuntu 24.04 in Dell Latitude 7340
tags:
  - ubuntu
  - broadcom
  - fingerprint
  - camera
  - kde
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Everything I know about enabling my webcam in Kubuntu on Dell laptop.
---

```bash
sudo add-apt-repository ppa:oem-solutions-group/intel-ipu6
sudo apt install libcamhal0 libcamhal-ipu6ep0 gstreamer1.0-icamera v4l2-relayd
sudo apt install intel-ipu6-dkms libcamhal0 libcamhal-ipu6ep0 libipu6ep
sudo ln -s /lib/firmware/intel/ipu/* /lib/firmware/intel/
```

Via the "Software & Updates" GUI Application > Additional Drivers Tab select:
Using HAL library for MIPI camera through Intel IPU6 from libcamhal0 (open source)

Reboot.

Notes:
- Camera in Kamoso wasn't working, but VLC and browsers were working.