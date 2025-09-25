---
layout: single
title: "Install GEMINI CLI, OpenCode CLI, Qwen Code CLI with Bun Runtime, and Crush CLI with Golang"
tags:
  - gemini
  - opencode
  - qwen
  - bun
  - cli
  - ai
  - tools
  - golang
  - crush
  - data-processing
categories:
  - tech
  - ai
  - tools
excerpt: "How to install GEMINI CLI, OpenCode CLI, and Qwen Code CLI using Bun runtime, plus Crush CLI for data processing using Golang."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:
- Install AI CLI tools using their respective methods
- Configure with API keys and verify installation
- Use for AI-assisted development and data processing

# Step-by-Step Installation Guide

## Step 1: Install Prerequisites

### Install Bun Runtime

```bash
curl -fsSL https://bun.sh/install | bash
```

### Install Golang (for Crush CLI)

```bash
# Ubuntu/Debian
sudo apt install golang-go

# macOS
brew install go

# Windows
choco install golang
```

## Step 2: Install AI CLI Tools

### Install GEMINI CLI

```bash
npm install -g @google/gemini-cli
```

### Install OpenCode CLI

```bash
curl -fsSL https://opencode.ai/install | bash
```

### Install Qwen Code CLI

```bash
npm install -g @qwen-code/qwen-code@latest
```

### Install Crush CLI

```bash
go install github.com/liljencrantz/crush@latest
```

## Step 3: Verify Installation

```bash
gemini --version
opencode --version
qwen --version
crush --version
```

## Step 4: Configure API Keys

```bash
# GEMINI CLI
gemini config set api-key YOUR_GOOGLE_API_KEY

# OpenCode CLI
opencode auth login

# Qwen Code CLI
qwen config set api-key YOUR_ALIBABA_API_KEY
```

## Step 5: Basic Usage

### GEMINI CLI

```bash
gemini ask "Explain async/await in JavaScript"
```

### OpenCode CLI

```bash
opencode "Generate a React component"
```

### Qwen Code CLI

```bash
qwen "Refactor this function"
```

### Crush CLI

```bash
echo "name,age\nAlice,30\nBob,25" | crush "select name, age from input where age > 20"
```


# References

- [GEMINI CLI GitHub](https://github.com/google-gemini/gemini-cli)
- [OpenCode CLI Website](https://opencode.ai)
- [Qwen Code CLI GitHub](https://github.com/QwenLM/qwen-code)
- [Bun Runtime](https://bun.sh)
- [Crush CLI GitHub](https://github.com/liljencrantz/crush)
