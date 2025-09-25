---
layout: single
title: "Setting Up vLLM with RTX 3090 eGPU on Laptop"
date: 2025-09-19
categories: [tutorial, ai, gpu]
tags: [vllm, rtx3090, egpu, laptop, cuda, nvidia]
excerpt: "Complete guide to install and configure vLLM with RTX 3090 eGPU on laptop using Docker and NVIDIA drivers."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/egpu-setup.png
  overlay_image: /assets/2025/09/egpu-setup.png
---

**TL;DR**:
- Set up RTX 3090 eGPU hardware connection
- Install NVIDIA drivers and CUDA toolkit
- Configure rootless Docker with GPU support
- Run vLLM container with GPU acceleration
- Test inference with your preferred models

# Prerequisites

### Hardware Requirements

```bash
# Verify eGPU connection
lspci | grep NVIDIA
```

### System Requirements

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install build-essential

# Docker (rootless)
curl -fsSL https://get.docker.com/rootless | sh
export PATH=$HOME/bin:$PATH
export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock

# NVIDIA drivers
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt install nvidia-driver-470

# CUDA toolkit
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo ./cuda_11.8.0_520.61.05_linux.run
```

## NVIDIA Drivers

### Installation

```bash
# Add NVIDIA PPA
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update

# Install NVIDIA driver
sudo apt install nvidia-driver-470
```

### Verify Installation

```bash
nvidia-smi
```

## CUDA Toolkit

### Installation

```bash
# Download CUDA 11.8
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run

# Make executable and run installer
chmod +x cuda_11.8.0_520.61.05_linux.run
sudo ./cuda_11.8.0_520.61.05_linux.run
```

### Verify Installation

```bash
nvcc --version
```

## Rootless Docker

### Installation

```bash
# Install rootless Docker
curl -fsSL https://get.docker.com/rootless | sh
export PATH=$HOME/bin:$PATH
export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock

# Add to shell profile
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
echo 'export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock' >> ~/.bashrc

# Start Docker daemon
systemctl --user start docker
```

### NVIDIA Container Toolkit Setup

```bash
# Install NVIDIA Container Toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/experimental/$distribution/libnvidia-container.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# Configure for rootless Docker
sudo nvidia-ctk runtime configure --runtime=docker --no-cgroups
sudo nvidia-ctk runtime configure --runtime=docker --config=$HOME/.config/docker/daemon.json
systemctl --user restart docker
```

### Test GPU Access

```bash
# Test NVIDIA Container Toolkit
docker run --rm --gpus all nvidia/cuda:11.8-base-ubuntu20.04 nvidia-smi
```

## vLLM

### Docker Installation

```bash
# Pull vLLM image
docker pull vllm/vllm-openai:latest

# Run vLLM server
docker run --gpus all --shm-size 1g \
  -p 8000:8000 \
  -v $HOME/.cache/huggingface:/root/.cache/huggingface \
  vllm/vllm-openai:latest \
  --model microsoft/DialoGPT-medium \
  --gpu-memory-utilization 0.9
```

### Native Installation

```bash
# Install Python environment
sudo apt install python3.8 python3.8-venv
python3 -m venv vllm-env
source vllm-env/bin/activate

# Install vLLM
pip install vllm
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Configuration

```bash
# Environment variables
export CUDA_VISIBLE_DEVICES=1
export PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:512
```

### Basic Usage

```bash
# Test API endpoint
curl -X POST "http://localhost:8000/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "microsoft/DialoGPT-medium",
    "messages": [{"role": "user", "content": "Hello"}],
    "max_tokens": 100
  }'
```

### Performance Monitoring

```bash
# Monitor GPU usage
watch -n 1 nvidia-smi
```

# References

- [vLLM GitHub](https://github.com/vllm-project/vllm)
- [vLLM Documentation](https://vllm.readthedocs.io/)
- [NVIDIA Drivers](https://www.nvidia.com/drivers)
- [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)
- [Rootless Docker](https://docs.docker.com/engine/security/rootless/)
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/)
- [eGPU Setup Guide](https://egpu.io/)
