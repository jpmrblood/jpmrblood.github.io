---
layout: single
title: "Install GEMINI CLI, OpenCode CLI, and Qwen Code CLI with Bun Runtime"
tags:
  - gemini
  - opencode
  - qwen
  - bun
  - cli
  - ai
  - tools
categories:
  - tech
  - ai
  - tools
excerpt: "How to install GEMINI CLI, OpenCode CLI, and Qwen Code CLI using Bun runtime for enhanced AI coding assistance."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:
- **Bun Runtime**: Fast all-in-one JavaScript runtime and package manager
- **GEMINI CLI**: Google's AI assistant for command-line tasks
- **OpenCode CLI**: AI coding assistant with project context awareness
- **Qwen Code CLI**: Alibaba's AI coding assistant with MCP integration
- **Unified Installation**: All tools installed globally using `bun`

# Installing AI CLI Tools with Bun Runtime

Bun is a fast all-in-one JavaScript runtime with a built-in package manager that's perfect for installing and running modern AI CLI tools. This guide covers installing three powerful AI coding assistants using Bun's global installation feature.

## What is Bun?

Bun is an incredibly fast JavaScript runtime, package manager, bundler, and test runner built from scratch using the Zig programming language. It's designed to be a drop-in replacement for Node.js with significantly better performance.

## Installing Bun (Prerequisites)

If you haven't installed Bun yet, here's how to get it:

```bash
# Install Bun using the official installer
curl -fsSL https://bun.sh/install | bash

# Or using npm (if you already have Node.js)
npm install -g bun

# Verify installation
bun --version
```

## Installing GEMINI CLI

GEMINI CLI is Google's command-line interface for interacting with Gemini AI models. It provides a powerful way to leverage Google's AI capabilities directly from your terminal.

```bash
# Install GEMINI CLI globally using Bun
bun i -g @google/gemini-cli@latest

# Verify installation
gemini-cli --version

# Configure with your Google API key
gemini-cli config set api-key YOUR_GOOGLE_API_KEY
```

### VSCode Integration for GEMINI CLI

- Install "Gemini CLI Companion" extension from VSCode marketplace
- Use command line: `gemini-cli` in integrated terminal
- Enhanced integration with VSCode workspace and editor

### Context File for GEMINI CLI

GEMINI CLI uses `GEMINI.md` for Google Gemini-specific configuration and project context.

## Installing OpenCode CLI

OpenCode CLI is an AI coding assistant that understands your project context and can help with code generation, refactoring, and explanation.

```bash
# Install OpenCode CLI globally using Bun
bun i -g opencode-ai

# Verify installation
opencode --version

# Configure with your preferred AI provider
opencode config set provider openai
opencode config set api-key YOUR_OPENAI_API_KEY
```

### VSCode Integration for OpenCode CLI

- Install "OpenCode" extension from VSCode marketplace
- Use command line: `opencode` in integrated terminal
- Access via VSCode command palette or extension UI

### Context File for OpenCode CLI

OpenCode CLI uses `AGENTS.md` for project-specific guidelines, build commands, and code style conventions.

## Installing Qwen Code CLI

Qwen Code CLI is Alibaba's AI coding assistant that can be integrated with MCP (Model Context Protocol) for enhanced context management.

```bash
# Install Qwen Code CLI globally using Bun
bun i -g qwen-code

# Verify installation
qwen-code --version

# Configure with your preferred model
qwen config set model qwen-plus
qwen config set api-key YOUR_ALIBABA_API_KEY
```

### VSCode Integration for Qwen Code CLI

- Use command line: `qwen-code` in integrated terminal
- No dedicated VSCode plugin needed
- Works directly in VSCode terminal

### Context File for Qwen Code CLI

Qwen Code CLI uses `QWEN.md` for Alibaba Qwen model configuration and project-specific instructions.

## Quick Start Guide

1. Install all tools globally with Bun:
   ```bash
   bun i -g @google/gemini-cli@latest opencode-ai qwen-code
   ```

2. Configure each tool with your respective API keys:
   ```bash
   gemini-cli config set api-key YOUR_GOOGLE_API_KEY
   opencode config set api-key YOUR_OPENAI_API_KEY
   qwen config set api-key YOUR_ALIBABA_API_KEY
   ```

3. Open your project in VSCode or any terminal

4. Use the tools in your terminal:
   ```bash
   # Using GEMINI CLI
   gemini-cli ask "Explain how to use async/await in JavaScript"
   
   # Using OpenCode CLI
   opencode "Generate a React component for a todo list"
   
   # Using Qwen Code CLI
   qwen-code "Refactor this function to be more efficient"
   ```

## Benefits of Using Bun for AI CLI Tools

1. **Speed**: Bun's package manager is significantly faster than npm or yarn
2. **Consistency**: All tools installed in one unified environment
3. **Efficiency**: Bun's runtime is optimized for performance
4. **Simplicity**: Single command installation for all tools

## Tips and Best Practices

- Use `bun i -g` for global installation of CLI tools
- All tools work seamlessly in VSCode terminal
- Keep your API keys secure using environment variables:
  ```bash
  export GOOGLE_API_KEY="your-google-api-key"
  export OPENAI_API_KEY="your-openai-api-key"
  export QWEN_API_KEY="your-alibaba-api-key"
  ```

- Update all tools regularly:
  ```bash
  bun update -g @google/gemini-cli@latest opencode-ai qwen-code
  ```

## Troubleshooting Common Issues

### Permission Issues

If you encounter permission issues during installation:

```bash
# Install with sudo if needed (not recommended)
sudo bun i -g @google/gemini-cli@latest

# Or configure Bun to install globally without sudo
mkdir ~/.bun/bin
export PATH="$HOME/.bun/bin:$PATH"
```

### Version Conflicts

To check installed versions and resolve conflicts:

```bash
# List all globally installed packages
bun pm ls -g

# Remove and reinstall if needed
bun remove -g @google/gemini-cli
bun i -g @google/gemini-cli@latest
```

### API Key Configuration

Ensure your API keys are correctly configured:

```bash
# Check current configuration
gemini-cli config list
opencode config list
qwen config list

# Reset configuration if needed
gemini-cli config reset
```

## Conclusion

Installing GEMINI CLI, OpenCode CLI, and Qwen Code CLI with Bun provides a fast and efficient way to access powerful AI coding assistants from your command line. With Bun's superior performance and unified package management, you can quickly set up and start using these tools to enhance your development workflow.

Each tool offers unique capabilities:
- **GEMINI CLI** for Google's advanced AI models
- **OpenCode CLI** for context-aware coding assistance
- **Qwen Code CLI** for Alibaba's Qwen models with MCP integration

With these tools installed globally via Bun, you'll have a powerful AI-assisted development environment at your fingertips.