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
- Install each AI CLI tool individually with proper commands
- Configure API keys for each tool separately
- Use tools for AI-assisted development and data processing

# Tool-by-Tool Installation Guide

## Prerequisites

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

## GEMINI CLI

### Installation

```bash
npm install -g @google/gemini-cli
```

### Verify Installation

```bash
gemini --version
```

### Configuration

```bash
gemini config set api-key YOUR_GOOGLE_API_KEY
```

### Basic Usage

```bash
gemini ask "Explain async/await in JavaScript"
```

## OpenCode CLI

### Installation

```bash
curl -fsSL https://opencode.ai/install | bash
```

### Verify Installation

```bash
opencode --version
```

### Configuration

```bash
opencode auth login
```

### Basic Usage

```bash
opencode "Generate a React component"
```

## Qwen Code CLI

### Installation

```bash
npm install -g @qwen-code/qwen-code@latest
```

### Verify Installation

```bash
qwen --version
```

### Configuration

```bash
qwen config set api-key YOUR_ALIBABA_API_KEY
```

### Basic Usage

```bash
qwen "Refactor this function"
```

## Crush CLI

### Installation

```bash
go install github.com/liljencrantz/crush@latest
```

### Verify Installation

```bash
crush --version
```

### Basic Usage

```bash
echo "name,age\nAlice,30\nBob,25" | crush "select name, age from input where age > 20"
```


# References

- [GEMINI CLI GitHub](https://github.com/google-gemini/gemini-cli)
- [OpenCode CLI Website](https://opencode.ai)
- [Qwen Code CLI GitHub](https://github.com/QwenLM/qwen-code)
- [Bun Runtime](https://bun.sh)
- [Crush CLI GitHub](https://github.com/liljencrantz/crush)
