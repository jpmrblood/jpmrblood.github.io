---
layout: single
title: "Compiling Memory MCP Server Without CUDA"
date: 2025-09-23
tags:
  - linux
  - mcp
  - memory
  - compilation
  - cpu-only
  - tutorial
categories:
  - linux
  - development
excerpt: "A step-by-step guide to compiling and installing MCP Memory Service on CPU-only systems without CUDA support."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/compiling-cpu.png
  overlay_image: /assets/2025/09/compiling-cpu.png
---

**TL;DR**:
- Clone the MCP Memory Service repository
- Install CPU-only Python dependencies
- Configure SQLite-vec storage backend
- Test compilation and basic functionality

## Why Compile Without CUDA

MCP Memory Service supports GPU acceleration via CUDA for improved performance, but many systems lack CUDA-compatible GPUs (most laptops, ARM devices, and some servers). This guide focuses on CPU-only compilation and installation, providing full functionality while maintaining reasonable performance through optimized CPU libraries.

## Enhanced Fork with CPU Support

**Note**: For optimized CPU-only compilation without CUDA dependencies, consider using my enhanced fork at [`jpmrblood/mcp-memory-service`](https://github.com/jpmrblood/mcp-memory-service), which includes dedicated CPU build profiles and Dockerfile.cpu support, with critically important pyproject.toml optimizations detailed below.

**Key improvements vs original doobidoo/mcp-memory-service**:
- **Added Dockerfile.cpu**: Docker container optimized for CPU-only deployments
- **Added .specify/ directory**: Contains templates and scripts for project management:
  - Bash scripts for development automation (check-prerequisites.sh, setup-plan.sh, create-new-feature.sh, update-agent-context.sh)
  - Memory constitution file for project guidelines
  - Templates for plans, specifications, tasks, and agent files
- **Modified .gitignore**: Added .specify directory to ignore development templates

## pyproject.toml Changes (Most Important)

The most critical improvement in the fork is the enhanced `pyproject.toml` configuration that enables true CPU-only installations:

### Added CPU Dependency Profile

```toml
[project.optional-dependencies]
cpu = [
    "sentence-transformers[onnx]>=2.2.2",
    "torch>=2.0.0"
]
```

This creates an `--extra cpu` installation option that:
- Forces CPU-only PyTorch without CUDA dependencies
- Uses ONNX-optimized sentence transformers for better performance
- Eliminates GPU package bloat from CPU installations

### Astral UV Integration

```toml
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", extra = "cpu" }
]
```

This sophisticated UV configuration:
- Defines a custom PyTorch CPU index that never pulls CUDA packages
- Automatically switches PyTorch to CPU-only wheels when using `--extra cpu`
- Ensures deterministic CPU-only builds regardless of system configuration
- Prevents accidental CUDA package installation

### Usage with UV

```bash
# CPU-optimized installation (recommended)
uv sync --extra cpu
```

Or,

```bash
uv pip install ".[cpu]"
```

This configuration eliminates the common issue where `pip install torch` automatically pulls CUDA-enabled wheels even on CPU-only systems, saving disk space and preventing compatibility issues.

## Prerequisites


My system is using Python3.13, so I need to install Python 3.12 or less. SO I install 3.11:

```bash
sudo apt update
sudo apt install python3.11 python3.11-venv python3-pip git build-essential
```

## Installation Steps

### 1. Clone the Repository

Clone the latest MCP Memory Service code:

```bash
git clone https://github.com/jpmrblood/mcp-memory-service.git
cd mcp-memory-service
```

### 2. Create Virtual Environment

Set up an isolated Python environment:

```bash
uv venv
source venv/bin/activate
```

### 3. Install CPU-Only Dependencies

Install dependencies using Astral UV for optimized CPU-only compilation:

```bash
# Install with CPU profile using uv
uv sync --extra cpu
```

Or/then,

```bash
# For compiling, enable CPU installation
uv pip install ".[cpu]"
```

### 4. Verify CPU Installation

Ensure the installation uses CPU-only libraries:

```bash
python3 -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'PyTorch version: {torch.__version__}')"
```

Expected output should show `CUDA available: False`.

### 5. Download Model for Embedding

Let's install hugging-face cli:

```bash
uv pip install -U "huggingface_hub[cli]"
```

Download the file:

```bash
uv run hf download sentence-transformers/all-MiniLM-L6-v2
```

### 7. Create a running script

I want to make this running as a service because I have multiple free clients that burn tokens easily.

In `~/.local/bin/run-mcp-memory.sh`

```bash
#!/bin/bash
# run-mcp-memory
export MCP_HTTP_ENABLED=true
export MCP_SERVER_HOST=0.0.0.0
export MCP_SERVER_PORT=8000
export MCP_MEMORY_STORAGE_BACKEND=sqlite_vec
export MCP_COMMAND="uv --directory=$HOME/mcp-memory-service run memory server"

npx -y supergateway --stdio "${MCP_COMMAND}" -outputTransport streamableHttp --port 8000

```

Yes, I run this using supergateway, a gateway that converts stdio MCPs into HTTP streaming and SSE.

Make the script executable:

```bash
chmod +x ~/.local/bin/run-mcp-memory.sh
```

It can run well now.

### 7. Run as server

```bash
run-mcp-memory.sh &
```

### 6. Install to the Editors

```bash
gemini mcp add -e sse -s user memory http://localhost:8000/sse
```

## References

- [MCP Memory Service GitHub Repository](https://github.com/doobidoo/mcp-memory-service)
- [PyTorch CPU Installation Guide](https://pytorch.org/get-started/locally/)
- [Using uv with PyTorch](https://docs.astral.sh/uv/guides/integration/pytorch/)
- [SQLite-vec Documentation](https://github.com/asg017/sqlite-vec)

Compiling MCP Memory Service without CUDA provides a lightweight, accessible solution for CPU-only environments while maintaining full compatibility with Claude's MCP protocol.
