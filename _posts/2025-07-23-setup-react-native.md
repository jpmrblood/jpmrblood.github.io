---
layout: single
title: Setup React Native without Android Studio
tags:
  - Android
  - react
  - native
  - sdk
categories:
  - linux
  - android
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2017/02/android-recharged.png
  overlay_image: /assets/2017/02/android-recharged.png
excerpt: Setup React Native without Android Studio
---

Installing Android Studio would be the best solution. However, not everyone has the same budget as you who can afford good laptop/PC. My computer screaming for mercy everytime I installed Android Studio. So, I go with VSCodium.

I'm on Fedora 42 but these can be applied to any other distro with some slight modification.

# Prerequisites

We can setup this later, but just put it here before I forgot.

```
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/platform-tools

echo "export ANDROID_HOME=\$HOME/Android/Sdk" >> ~/.bashrc
echo "export PATH=\$PATH:\$ANDROID_HOME/emulator" >> ~/.bashrc 
echo "export PATH=\$PATH:\$ANDROID_HOME/platform-tools" >> ~/.bashrc
```

# Java SDK

Current Android uses JDK 17 although [React Native documentation said JDK 11](https://reactnative.dev/docs/0.71/environment-setup). This is because the [JDK related to supported JDK version by Gradle](https://developer.android.com/build/jdks#jdk-gradle). Well, Gradle 8.5 supports JDK 21 and Android Studio uses `jbr-21` [(Jetbrains' JDK)](https://github.com/JetBrains/JetBrainsRuntime/releases). Let me download and install it.

```bash
sudo dnf install java-21-openjdk-devel
```

# Install Android Commandline Tools

Download it:
```bash
export DOWNLOAD_DIR=${PWD}
wget https://dl.google.com/android/repository/commandlinetools-linux-13114758_latest.zip
```

Setup the place

```bash
mkdir $ANDROID_HOME/cmdline-tools
cd $ANDROID_HOME/cmdline-tools
unzip ${DOWNLOAD_DIR}$/commandlinetools-linux-13114758_latest.zip && mv cmdline-tools latest
```

Setup PATH for convenient:
```bash
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin

echo "export PATH=\$PATH:\$ANDROID_HOME/cmdline-tools/latest/bin" >> ~/.bashrc 

```

With this, you are now can setup Android.


# Install Android SDK

Let's install Android SDK latest version. See with:

```bash
sdkmanager --list
```

Copy the name you want to install later with single quote.

## Android Platform

```bash
sdkmanager --install 'platforms;android-36'
```

## Build Tools

```bash
sdkmanager --install 'build-tools;36.0.0'
```


## Emulator
```bash
sdkmanager --install 'emulator'
```

# Run an emulator

I'm targeting Google Pixel 2 with Android Q/10 (API 29). Let's 

## Install the image 

FInd it
```bash

sdkmanager --list | grep android-29

```

Install the image first:

```bash
sdkmanager --install 'system-images;android-29;google_apis_playstore;x86_64'
```

Create the AVD:
```bash
avdmanager --verbose create avd --force --name "test_phone" --package "system-images;android-29;google_apis_playstore;x86_64" --tag "google_apis_playstore" --abi "x86_64" --device "pixel_2"
```

Run the emulator:
```bash
emulator -avd test_phone &
```

# Test Drive create A Project

Create the default project
```bash
bun x @react-native-community/cli@latest init AwesomeProject
```


Run it:
```bash
cd AwesomeProject
bun android
```

I tempted to setup Nativewind. Later.




