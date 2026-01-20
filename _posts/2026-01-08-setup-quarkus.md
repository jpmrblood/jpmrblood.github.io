---
layout: single
title: Install/Upgrade Quarkus Locally
tags:
  - java
  - quarkus
  - sdkman
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2017/09/java2.png
  overlay_image: /assets/2017/09/java2.png
excerpt: Upgrading Golang to newer version
---
Here's the way.

# Install SDKMAN for quality of life

```bash
curl -s "https://get.sdkman.io" | bash
```

Load the env in an existing environment:

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

# Install GraalVM

You know you want Quarkus because of GraalVM.
See the latest version:

```bash
sdk list java | grep mandrel
```

Install the latest:

```bash
sdk install java 25.0.1.r25-mandrel
```

# Install Quarkus CLI for hacker effect

NOTE: we know you want to setup Quarkus from the starter page, right?

```bash
sdk install quarkus
```

# Create a Quarkus project

```bash
quarkus create app com.example:self-service \
  --gradle \
  --java=25 \
  --extension="resteasy-reactive,qute,hibernate-orm-panache"
```