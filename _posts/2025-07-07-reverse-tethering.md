---
layout: single
title: Generate 32 Random Characters
tags:
  - tethering
  - usb
  - gnirehtet
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Reverse tethering via USB
---
I want to connect my phone to my laptop.

# Prepare the phone

1. Install adb on Ubuntu

```bash
sudo apt install android-tools-adb
```

2. Enable Developer Options
`Settings` → `About Phone` → Tap Build Number 7 times.

Go to `System` → `Developer Options`

Enable USB Debugging

3. Plug in phone via USB → Approve ADB debugging
Run:

```bash
adb devices
```

Make sure phone appears in the list.

# Use the app

```bash
wget https://github.com/Genymobile/gnirehtet/releases/download/v2.5.1/gnirehtet-rust-linux64-v2.5.1.zip
unzip gnirehtet-rust-linux64-v2.5.1.zip
cd gnirehtet-rust-linux64/
chmod +x gnirehtet
```


Run

```bash
./gnirehtet

```