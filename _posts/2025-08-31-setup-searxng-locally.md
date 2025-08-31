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

SearxNG is a free internet metasearch engine which aggregates results from more than 70 search services while respecting your privacy. It's a privacy-respecting, hackable metasearch engine that can be self-hosted. This guide walks through the step-by-step setup of SearxNG on a Linux system using Docker for a simple and isolated installation.

## Prerequisites

- A Linux machine (e.g., CentOS 7/8, RHEL 8, or Ubuntu 20.04+). I'm using Fedora 42.
- Internet access
- Administrative privileges to install software via `sudo`
- Docker installed on your system

---

## Step 1: Install Docker

Begin by installing Docker, which is required for running SearxNG in an isolated container environment.

```bash
sudo yum install docker
```

> 📝 Explanation of parameters:
> - `sudo`: Executes the command with superuser privileges, required for installing system packages.
> - `yum`: The package manager for RPM-based Linux distributions like Fedora.
> - `install`: The subcommand to install a specified package.
> - `docker`: The name of the package to install, Docker container runtime.

For Ubuntu/Debian systems, use:
```bash
sudo apt update && sudo apt install docker.io
```

---

## Step 2: Start and Enable Docker Service

Start the Docker service and enable it to start automatically on boot.

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

> 📝 Explanation of parameters:
> - `systemctl`: Command to control the systemd system and service manager.
> - `start`: Subcommand to start a service.
> - `enable`: Subcommand to enable a service to start automatically at boot.
> - `docker`: The name of the service to control.

---

## Step 3: Create SearxNG Configuration Directory

Create a directory to store SearxNG configuration files and data.

```bash
mkdir -p ~/searxng
cd ~/searxng
```

> 📝 Explanation of parameters:
> - `mkdir`: Command to create directories.
> - `-p`: Option to create parent directories as needed.
> - `~/searxng`: Path to create the SearxNG directory in your home folder.
> - `cd`: Command to change the current directory.
> - `~/searxng`: Path to navigate to the newly created directory.

---

## Step 4: Download SearxNG Settings Template

Download the default SearxNG settings template to customize your instance.

```bash
curl -O https://raw.githubusercontent.com/searxng/searxng/master/searxng/settings.yml
```

> 📝 Explanation of parameters:
> - `curl`: Command-line tool for transferring data with URLs.
> - `-O`: Option to write output to a file named as the remote file.
> - `https://raw.githubusercontent.com/searxng/searxng/master/searxng/settings.yml`: URL to the SearxNG settings template.

---

## Step 5: Customize SearxNG Settings (Optional)

Edit the settings.yml file to customize your SearxNG instance. For basic setup, you can skip this step, but for production use, consider changing the secret key:

```bash
# Generate a secret key
openssl rand -hex 32
```

Then edit the settings.yml file to replace the default secret key:
```bash
nano settings.yml
```

Find the `server.secret_key` line and replace the value with your generated key.

---

## Step 6: Create Docker Compose File

Create a docker-compose.yml file to define the SearxNG service:

```bash
cat > docker-compose.yml << 'EOF'
version: '3.7'

services:
  searxng:
    image: searxng/searxng:latest
    container_name: searxng
    ports:
      - "8080:8080"
    volumes:
      - ./settings.yml:/etc/searxng/settings.yml:ro
    environment:
      - SEARXNG_BASE_URL=https://localhost/searxng/
    restart: unless-stopped
EOF
```

> 📝 Explanation of parameters:
> - `cat`: Command to concatenate and display files.
> - `> docker-compose.yml`: Redirects output to create/overwrite the docker-compose.yml file.
> - `<< 'EOF'`: Heredoc syntax to provide multi-line input.
> - `version: '3.7'`: Docker Compose file format version.
> - `image: searxng/searxng:latest`: Specifies the SearxNG Docker image to use.
> - `container_name: searxng`: Sets a custom name for the container.
> - `ports: - "8080:8080"`: Maps port 8080 on host to port 8080 in container.
> - `volumes: - ./settings.yml:/etc/searxng/settings.yml:ro`: Mounts local settings file into container as read-only.
> - `environment: - SEARXNG_BASE_URL=https://localhost/searxng/`: Sets the base URL environment variable.
> - `restart: unless-stopped`: Automatically restarts the container unless explicitly stopped.

---

## Step 7: Start SearxNG Service

Launch the SearxNG service using Docker Compose:

```bash
docker-compose up -d
```

> 📝 Explanation of parameters:
> - `docker-compose`: Command to manage multi-container Docker applications.
> - `up`: Creates and starts containers.
> - `-d`: Runs containers in the background (detached mode).

> 🚀 This command pulls the SearxNG Docker image and starts the service. The first run may take a few minutes to download the image.

---

## Step 8: Verify SearxNG is Running

Check that the SearxNG container is running properly:

```bash
docker-compose ps
```

> 📝 Explanation of parameters:
> - `docker-compose`: Command to manage multi-container Docker applications.
> - `ps`: Lists containers.

You should see output similar to:
```
NAME      IMAGE                  COMMAND                  SERVICE   CREATED        STATUS        PORTS
searxng   searxng/searxng:latest "/sbin/tini -- /usr/…"   searxng   1 minute ago   Up 1 minute   0.0.0.0:8080->8080/tcp
```

---

## Step 9: Access SearxNG Web Interface

Open your web browser and navigate to:

```
http://localhost:8080
```

> 🌐 The SearxNG web interface will be available at this address.

> ✅ You can now perform private searches without your queries being tracked by major search engines.

---

## Additional Configuration Options

### Changing the Port

To use a different port, modify the `ports` section in docker-compose.yml:
```yaml
ports:
  - "9090:8080"
```

Then restart the service:
```bash
docker-compose down
docker-compose up -d
```

### Persistent Data Storage

To persist search history and settings, add a volume for the data directory:
```yaml
volumes:
  - ./settings.yml:/etc/searxng/settings.yml:ro
  - ./data:/var/lib/searxng:rw
```

Create the data directory:
```bash
mkdir data
```

---

## Use Cases

- **Privacy-First Search**: Ideal for users who want to avoid tracking by major search engines.
- **Ad-Free Experience**: Enjoy a clean, ad-free search experience.
- **Customizable Interface**: Modify the look and feel to suit your preferences.
- **Multiple Search Engines**: Aggregate results from various sources in one place.
- **Educational Use**: Great for learning about search engines and privacy technologies.

---

## Troubleshooting Tips

- **Port Conflict**: If port 8080 is already in use, change it in the docker-compose.yml file.
- **Docker Permission Denied**: Add your user to the docker group with `sudo usermod -aG docker $USER` and log out/in.
- **Container Not Starting**: Check logs with `docker-compose logs searxng` for error messages.
- **Configuration Issues**: Ensure the settings.yml file has proper YAML formatting.
- **Network Issues**: Verify internet connectivity and firewall settings.
- **Service Not Accessible**: Confirm no firewall is blocking the port. Use `curl http://localhost:8080` to test connectivity.
- **Performance Issues**: For high-traffic instances, consider adding resource limits to docker-compose.yml.

---

## Updating SearxNG

To update to the latest version of SearxNG:

```bash
docker-compose pull
docker-compose up -d
```

> 🔄 This command pulls the latest SearxNG Docker image and restarts the service with the updated version.

---

## Conclusion

SearxNG provides an excellent way to maintain your privacy while searching the web. By self-hosting, you have complete control over your search data and can customize the experience to your needs. The Docker-based setup makes installation and maintenance straightforward while providing an isolated environment for the service.

With this setup, you can enjoy private, ad-free search results from multiple engines without compromising your privacy.