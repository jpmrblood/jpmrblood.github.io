---
layout: single
title: Setting Up SearxNG Locally for Private Search
tags:
  - searxng
  - privacy
  - search
  - docker
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
excerpt: Learn how to set up SearxNG, a privacy-respecting metasearch engine, on your local machine
---

# Setting Up SearxNG Locally for Private Search

SearxNG is a free internet metasearch engine which aggregates results from more than 70 search services while respecting your privacy. It's a privacy-respecting, hackable metasearch engine that can be self-hosted. This guide walks through the step-by-step setup of SearxNG on a GNU/Linux system using Docker for a simple and isolated installation.

## Prerequisites

Before you begin, ensure your laptop meets the following requirements:

  * **Operating System**: Windows 10/11, macOS, or a Linux distribution.
  * **System Resources**: A **64-bit processor** is recommended. The application is lightweight, with a minimum of **512MB RAM** and around **300MB of storage** required for the Docker image itself. However, **2GB of RAM or more** is better for optimal performance.
  * **Software**: You must have **Docker** or **Podman** installed and running on your system. Docker is the most common choice and is available for all major operating systems.

## Installation Steps

### 1\. Install Docker

If you haven't already, download and install Docker Desktop from the official Docker website. Once installed, start the Docker application.

-----

### 2\. Get the SearXNG Docker Compose File

The easiest way to run SearXNG is by using the official `searxng-docker` repository, which includes a `docker-compose.yml` file to manage the SearXNG container and its dependencies (like Redis).

1.  Open your terminal or command prompt.
2.  Clone the repository using `git`:
    ```bash
    git clone https://github.com/searxng/searxng-docker.git
    ```
3.  Navigate into the new directory:
    ```bash
    cd searxng-docker
    ```

-----

### 3\. Generate a Secret Key

For security, you need to generate a unique secret key for your instance. This key is used for various cryptographic operations.

  * **On Linux/macOS**:
    ```bash
    sed -i "s|ultrasecretkey|$(openssl rand -hex 32)|g" searxng/settings.yml
    ```
  * **On Windows (PowerShell)**:
    ```powershell
    $secretKey = -join ((New-Object System.Security.Cryptography.RNGCryptoServiceProvider).GetBytes(32) | ForEach-Object { "{0:x2}" -f $_ })
    (Get-Content searxng/settings.yml) -replace 'ultrasecretkey', $secretKey | Set-Content searxng/settings.yml
    ```

-----

### 4\. Run SearXNG

With the secret key configured, you can now start the SearXNG service using Docker Compose.

1.  In your terminal, from the `searxng-docker` directory, run the following command:
    ```bash
    docker compose up -d
    ```
    This command will pull the necessary Docker images, create the containers, and run them in the background (`-d` for detached mode).
2.  To check if the containers are running, use:
    ```bash
    docker ps
    ```
    You should see `searxng` and `redis` listed as running containers.

-----

### 5\. Access Your Instance

Once the containers are up, your SearXNG instance will be accessible through your web browser.

  * Open your browser and navigate to **`http://localhost:8080`**.

You should now see the SearXNG search interface. You can begin searching and customize your settings, such as enabling or disabling specific search engines.