---
layout: single
title: "Install Astral UV with the Official Bash Script"
tags:
  - python
  - uv
  - astral
  - package-manager
  - rust
categories:
  - tech
  - python
  - tools
excerpt: "How to install Astral UV, the ultra-fast Python package manager written in Rust, using the official bash script method."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:
- **Astral UV**: Fast Python package manager written in Rust
- **10-100x faster**: Than pip
- **Single tool**: Replaces pip, pip-tools, pipx, poetry, pyenv, twine, and virtualenv
- **Official installation**: Use the bash script on macOS/Linux

# Astral UV: Fast Python Package Management

Astral UV is a fast Python package and project manager written in Rust. It serves as a drop-in replacement for multiple Python tools:

- **pip**: Package installation
- **pip-tools**: Dependency management
- **pipx**: Tool installation
- **poetry**: Project management
- **pyenv**: Python version management
- **twine**: Package publishing
- **virtualenv**: Virtual environments

## Why Use Astral UV?

### Performance

UV is significantly faster than traditional tools due to its Rust implementation:

- **10-100x faster** than pip for dependency resolution and installation
- Native binary execution without Python interpreter startup time
- Parallelized operations
- Minimal memory footprint

### Features

UV provides a complete Python development workflow:

- Dependency resolution with universal lockfiles
- Virtual environment management
- Script execution with inline dependency metadata
- Python version installation and switching
- Self-updating capabilities
- Cross-platform support (macOS, Linux, Windows)

## Installing Astral UV with the Official Bash Script

The recommended way to install UV on macOS and Linux systems is using the official standalone installer script.

### Installation Command

To install UV using the official bash script, run:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

This command:
1. Downloads the official installation script from Astral's servers
2. Executes it with `sh` to install the UV binary
3. Sets up your PATH automatically
4. Enables self-update functionality

### Windows Installation

For Windows users, use the PowerShell script:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Verifying Installation

Check that UV is properly installed:

```bash
uv --version
```

## Benefits of the Standalone Installer

Using the official bash script provides several advantages:

### 1. No Prerequisites
- No need for Rust or Python to be pre-installed
- Works on minimal systems
- Single binary installation

### 2. Self-Update Capability
Once installed via the script, UV can update itself:
```bash
uv self update
```

### 3. Optimized Binary
- Direct binary installation without Python interpreter overhead
- Faster startup times
- Better performance compared to PyPI installations

## Alternative Installation Methods

UV can also be installed via other methods:

### PyPI Installation
```bash
# With pip
pip install uv

# Or with pipx (recommended for tool installation)
pipx install uv
```

### Docker Usage
UV can also be used via Docker:
```dockerfile
FROM ghcr.io/astral-sh/uv:latest
```

## Getting Started with UV

After installation, you can start using UV for common tasks:

### Creating a Virtual Environment
```bash
uv venv myproject
source myproject/bin/activate
```

### Installing Packages
```bash
uv pip install requests numpy pandas
```

### Running Python Scripts
```bash
uv run python script.py
```

## Conclusion

Astral UV is a fast, comprehensive tool for Python development that replaces multiple traditional tools. Installing it with the official bash script provides the best experience with automatic updates and optimal performance.

Try it out by running the installation command and see the performance improvements in your Python projects.