---
layout: single
title: Install Kubuntu 24.04 in Dell Latitude 7340
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

I have a Dell Latitude 7340 laptop pre-installed with Windows 11. I want to install Kubuntu on the laptop because my workflow is using KDE on GNU/Linux. I want to get operation.

The reason why I chose Kubuntu is also because the laptop is listed as [supported hardware](https://ubuntu.com/certified/202212-30956/22.04%20LTS). Unfortunately, it is on 22.04. No problem for me, at least we know it was supported.

# Install LUKS2 on the whole partition

Not doing it. Just use the default partition from Kubuntu, a separated partition for `/boot`

# Install Fingerprint Driver

My fingerprint device from `lsusb`:

```
Bus 001 Device 005: ID 0a5c:5843 Broadcom Corp. 58200
```

Go to [Dell partner site in Canonical for Ubuntu 22.04](http://dell.archive.canonical.com/updates/pool/public/libf/libfprint-2-tod1-broadcom/) and select the latest one. I select [libfprint-2-tod1-broadcom_5.12.018-0ubuntu1~22.04.01_amd64.deb](http://dell.archive.canonical.com/updates/pool/public/libf/libfprint-2-tod1-broadcom/libfprint-2-tod1-broadcom_5.12.018-0ubuntu1~22.04.01_amd64.deb). Just install it directly from browser.


# Install Camera Driver

For camera device, I installed OEM kernel and IPU6 DKMS driver.

I'm installing a development repo:

```bash
sudo add-apt-repository ppa:oem-solutions-group/intel-ipu6
sudo apt update

```

There is an error if you are using [Ubuntu's generic kernel]](https://www.dell.com/support/kbdoc/en-uk/000225004/precision-5480-mobile-workstation-webcam-does-not-work-on-ubuntu-24-04). So, I installed OEM kernel.

```bash
sudo apt install linux-image-oem-24.04 linux-modules-ipu6-oem-24.04
```

If it's not working, use `linux-modules-ipu6-oem-24.04a`.

I also tried to install these packages:

```bash
sudo apt install gstreamer1.0-icamera intel-ipu6-dkms
```

I don't know which driver is being used. All I know my camera is working now.