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
- Install Astral UV using the official bash script.
- Verify installation with version check.
- Use UV for virtual environments and package management.

# Step-by-Step Guide to Install Astral UV

## Step 1: Install UV

Run the official bash script to install UV on macOS/Linux.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

For Windows, use PowerShell:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## Step 2: Verify Installation

Check that UV is installed correctly.

```bash
uv --version
```

## Step 3: Basic Usage

Create a virtual environment and install packages.

```bash
uv venv myproject
source myproject/bin/activate  # On Windows: myproject\Scripts\activate
uv pip install requests
```

## Usage Examples

### Basic Package Installation

Install packages in a virtual environment.

```bash
uv pip install numpy pandas matplotlib
```

### Running Scripts

Execute Python scripts with dependencies.

```bash
uv run python script.py
```

### Project Management

Initialize a new Python project.

```bash
uv init myproject
cd myproject
uv add requests
```

# References

- [Astral UV Official Documentation](https://docs.astral.sh/uv/)
- [Astral UV GitHub Repository](https://github.com/astral-sh/uv)