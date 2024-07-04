---
layout: single
title: Razer BlackShark V2 HS 2.4 on Fedora 40
tags:
  - fedora
  - headphone
  - linux
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to install Kubernetes
---
I got a new Razer BlackShark V2 HS 2.4. It can runs fine with Bluetooth connection. So, no complain there.

# Using The Dongle

Now, I bought the full price with the dongle. I want that dongle working in my GNU/Linux Fedora 40 laptop.

- I disable the Bluetooth and restart the headphone.
- I am using the cable that comes with the package. It seems the dongle is not working when using another cable.
- The dongle is active without any further settings.

Well, this is awkward. Lol, no need to tinker anything.

# The Supposed Tinkering

[The Open Razer Project](https://openrazer.github.io/#devices) provides a custom kernel where I want to have a hands on. I am on secure boot, so I wonder how would it be like to install the custom kernel. Well, no need.

The device is fully recognized in `dmesg`:

```bash
[ 1026.687797] usb 1-2: new full-speed USB device number 8 using xhci_hcd
[ 1026.819747] usb 1-2: not running at top speed; connect to a high speed hub
[ 1026.837803] usb 1-2: New USB device found, idVendor=1532, idProduct=0565, bcdDevice= 1.00
[ 1026.837818] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 1026.837825] usb 1-2: Product: Razer BlackShark V2 HS 2.4
[ 1026.837830] usb 1-2: Manufacturer: MediaTek Inc
[ 1026.837835] usb 1-2: SerialNumber: 0000000000000000
[ 1027.000390] input: MediaTek Inc Razer BlackShark V2 HS 2.4 as /devices/pci0000:00/0000:00:08.1/0000:06:00.3/usb1/1-2/1-2:1.3/0003:1532:0565.0003/input/input25
[ 1027.000710] input: MediaTek Inc Razer BlackShark V2 HS 2.4 Consumer Control as /devices/pci0000:00/0000:00:08.1/0000:06:00.3/usb1/1-2/1-2:1.3/0003:1532:0565.0003/input/input26
[ 1027.053277] input: MediaTek Inc Razer BlackShark V2 HS 2.4 as /devices/pci0000:00/0000:00:08.1/0000:06:00.3/usb1/1-2/1-2:1.3/0003:1532:0565.0003/input/input27
[ 1027.053668] input: MediaTek Inc Razer BlackShark V2 HS 2.4 as /devices/pci0000:00/0000:00:08.1/0000:06:00.3/usb1/1-2/1-2:1.3/0003:1532:0565.0003/input/input28
[ 1027.054243] hid-generic 0003:1532:0565.0003: input,hiddev96,hidraw0: USB HID v1.10 Device [MediaTek Inc Razer BlackShark V2 HS 2.4] on usb-0000:06:00.3-2/input3
```

It seems the dongle uses MediaTek chipset. MediaTek really has comes so far. Good for them.

The `lsusb` recognized this as:

```bash
Bus 001 Device 008: ID 1532:0565 Razer USA, Ltd Razer BlackShark V2 HS 2.4
```

Oh, well, the USB ID is not listed. No complain here, though. The dongle is working out of the box.

The FOSS has come so far today. Good job!