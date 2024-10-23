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
excerpt: Everything I know about installing Kubuntu on Dell laptop.
---

These are notes by [bryan977](https://github.com/intel/ipu6-drivers/issues/228#issuecomment-2276654389):


I upgraded to the 6.8.0-1010-oem kernel and my camera is still working. This thread has got quite long, so I figured posting a consolidated process and current status may be helpful for others.

Notes:

>    System: Dell XPS 13 9340 (2024 model)
>    Driver: Using HAL library for MIPI camera through Intel IPU6 from libcamhal0 (open source)
>    Process used to make it work:

```bash
sudo apt install linux-oem-24.04
sudo add-apt-repository ppa:oem-solutions-group/intel-ipu6
sudo apt install libcamhal0 libcamhal-ipu6ep0 gstreamer1.0-icamera v4l2-relayd
sudo apt install linux-modules-ipu6-oem-24.04 linux-oem-24.04
```

Via the "Software & Updates" GUI Application > Additional Drivers Tab select:
Using HAL library for MIPI camera through Intel IPU6 from libcamhal0 (open source)

I think the above is everything I did to make it work (I did document it in the thread above). I started with Ubuntu 24.04 running the generic kernel (I forget what version).


