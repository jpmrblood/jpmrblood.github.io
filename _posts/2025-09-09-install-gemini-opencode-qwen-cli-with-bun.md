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
- **Bun Runtime**: Fast all-in-one JavaScript runtime and package manager
- **GEMINI CLI**: Google's AI assistant for command-line tasks
- **OpenCode CLI**: AI coding assistant with project context awareness
- **Qwen Code CLI**: Alibaba's AI coding assistant with MCP integration
- **Crush CLI**: Data processing tool with SQL-like syntax
- **Unified Installation**: AI tools installed with `bun`, Crush CLI installed with `go`

# Installing AI CLI Tools with Bun Runtime and Crush CLI with Golang

Bun is a fast all-in-one JavaScript runtime with a built-in package manager that's perfect for installing and running modern AI CLI tools. This guide covers installing three powerful AI coding assistants using Bun's global installation feature, plus Crush CLI for data processing using Golang's package management.

## What is Bun?

Bun is an incredibly fast JavaScript runtime, package manager, bundler, and test runner built from scratch using the Zig programming language. It's designed to be a drop-in replacement for Node.js with significantly better performance.

## Installing Bun and Golang (Prerequisites)

If you haven't installed Bun yet, here's how to get it:

```bash
# Install Bun using the official installer
curl -fsSL https://bun.sh/install | bash

# Or using npm (if you already have Node.js)
npm install -g bun

# Verify installation
bun --version
```

For Crush CLI, you'll need Golang:

```bash
# Install Golang (varies by OS)
# On Ubuntu/Debian:
sudo apt install golang-go

# On macOS with Homebrew:
brew install go

# On Windows with Chocolatey:
choco install golang

# Verify installation
go version
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

## Installing Crush CLI with Golang

Crush CLI is a powerful data processing tool that allows you to manipulate structured data (CSV, JSON, Parquet, etc.) directly from the command line using SQL-like queries. Unlike the other AI tools covered in this guide, Crush CLI is written in Rust and installed using Golang tooling.

```bash
# Install Crush CLI using Go
go install github.com/liljencrantz/crush@latest

# Verify installation
crush --version

# Basic usage example
echo "name,age\nAlice,30\nBob,25" | crush "select name, age from input where age > 20"
```

### Features of Crush CLI

- **Multi-format Support**: Works with CSV, JSON, Parquet, Excel, and more
- **SQL-like Syntax**: Familiar query language for data manipulation
- **Streaming Processing**: Handles large datasets efficiently
- **Rich Function Library**: Built-in functions for data transformation
- **Extensible**: Write custom functions in Crush script

### Example Use Cases

```bash
# Convert CSV to JSON
crush "select * from input.csv" > output.json

# Filter and sort data
crush "select name, salary from employees.csv where salary > 50000 order by salary desc" > high_earners.csv

# Join multiple data sources
crush "select a.name, a.age, b.salary from employees.csv a join salaries.csv b on a.id = b.id" > employee_data.csv
```

### Integration with Other Tools

Since Crush CLI works with standard input/output streams, it integrates seamlessly with the other AI CLI tools:

```bash
# Use Crush to preprocess data before sending to Qwen Code CLI
crush "select description from issues.csv where status = 'open'" | qwen-code "Generate bug fix suggestions for these issues"

# Process AI-generated JSON with Crush
gemini-cli ask "List the top 10 programming languages with their popularity scores" --json | crush "select language, score from input order by score desc"
```

## Quick Start Guide

1. Install AI tools globally with Bun:
   ```bash
   bun i -g @google/gemini-cli@latest opencode-ai qwen-code
   ```

2. Install Crush CLI with Golang:
   ```bash
   go install github.com/liljencrantz/crush@latest
   ```

3. Configure each AI tool with your respective API keys:
   ```bash
   gemini-cli config set api-key YOUR_GOOGLE_API_KEY
   opencode config set api-key YOUR_OPENAI_API_KEY
   qwen config set api-key YOUR_ALIBABA_API_KEY
   ```

4. Open your project in VSCode or any terminal

4. Use the tools in your terminal:
   ```bash
   # Using GEMINI CLI
   gemini-cli ask "Explain how to use async/await in JavaScript"
   
   # Using OpenCode CLI
   opencode "Generate a React component for a todo list"
   
   # Using Qwen Code CLI
   qwen-code "Refactor this function to be more efficient"
   
   # Using Crush CLI for data processing
   echo "name,age\nAlice,30\nBob,25" | crush "select name, age from input where age > 20"
   ```

## Benefits of Using Bun for AI CLI Tools and Golang for Crush CLI

1. **Speed**: Bun's package manager is significantly faster than npm or yarn for JavaScript tools
2. **Consistency**: AI tools installed in one unified environment with Bun
3. **Efficiency**: Bun's runtime is optimized for performance, while Crush CLI provides efficient data processing
4. **Simplicity**: Single command installation for AI tools with Bun, straightforward installation for Crush CLI with Golang
5. **Complementary Capabilities**: AI tools for code generation and assistance combined with Crush CLI for data manipulation

## Tips and Best Practices

- Use `bun i -g` for global installation of JavaScript-based CLI tools
- Use `go install` for installing Golang-based tools like Crush CLI
- All tools work seamlessly in VSCode terminal
- Keep your API keys secure using environment variables:
  ```bash
  export GOOGLE_API_KEY="your-google-api-key"
  export OPENAI_API_KEY="your-openai-api-key"
  export QWEN_API_KEY="your-alibaba-api-key"
  ```

- Update all tools regularly:
  ```bash
  # Update Bun-based tools
  bun update -g @google/gemini-cli@latest opencode-ai qwen-code
  
  # Update Crush CLI
  go install github.com/liljencrantz/crush@latest
  ```

## Troubleshooting Common Issues

### Permission Issues

If you encounter permission issues during installation:

```bash
# Install Bun tools with sudo if needed (not recommended)
sudo bun i -g @google/gemini-cli@latest

# Install Crush CLI with sudo if needed (not recommended)
sudo go install github.com/liljencrantz/crush@latest

# Or configure Bun to install globally without sudo
mkdir ~/.bun/bin
export PATH="$HOME/.bun/bin:$PATH"

# Or configure Go to install binaries to a user directory
export GOPATH="$HOME/go"
export PATH="$GOPATH/bin:$PATH"
```

### Version Conflicts

To check installed versions and resolve conflicts:

```bash
# List all globally installed Bun packages
bun pm ls -g

# Remove and reinstall Bun tools if needed
bun remove -g @google/gemini-cli
bun i -g @google/gemini-cli@latest

# Check Crush CLI version
crush --version

# Reinstall Crush CLI if needed
go clean -modcache
go install github.com/liljencrantz/crush@latest
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

Installing GEMINI CLI, OpenCode CLI, and Qwen Code CLI with Bun provides a fast and efficient way to access powerful AI coding assistants from your command line. Crush CLI offers excellent data processing capabilities using Golang tooling. With these tools installed globally, you'll have a comprehensive command-line environment for AI assistance and data manipulation.

Each tool offers unique capabilities:
- **GEMINI CLI** for Google's advanced AI models
- **OpenCode CLI** for context-aware coding assistance
- **Qwen Code CLI** for Alibaba's Qwen models with MCP integration
- **Crush CLI** for powerful data processing with SQL-like syntax

With these tools installed globally via Bun and Golang, you'll have a powerful AI-assisted development and data processing environment at your fingertips.