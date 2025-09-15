---
layout: single
title: "Use GitHub Spec Kit with QWEN Code CLI"
tags:
   - ai
   - github
   - api
   - development
   - specifications
   - software-engineering
   - productivity
   - qwen
   - cli
categories:
  - tech
  - programming
  - ai-tools
excerpt: "Learn how to integrate GitHub Spec Kit with QWEN Code CLI for enhanced spec-driven development. Discover workflows that combine specification management with AI-powered coding assistance for more efficient software projects."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/github-spec-kit.png
  overlay_image: /assets/2025/09/github-spec-kit.png
---

**TL;DR**:
- Initialize a new Spec Kit project.
- Rename `.gemini` to `.qwen` for QWEN compatibility.
- Use Spec Kit commands in QWEN CLI for spec-driven development.

# Step-by-Step Integration Guide

## Step 1: Initialize Spec Project

Create a new project directory with GitHub Spec Kit configured for QWEN Code CLI.

### Option A: New Project Directory

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --ai gemini --script sh <PROJECT_NAME>
cd <PROJECT_NAME>
```

Replace `<PROJECT_NAME>` with your desired project name.

### Option B: Current Directory

Initialize Spec Kit in your current directory.

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --ai gemini --script sh --here
```

## Step 2: Configure QWEN Integration

After initialization, rename the `.gemini` directory to `.qwen` for QWEN CLI compatibility.

```bash
mv .gemini .qwen
```

Now, all GitHub Spec Kit commands are available in QWEN Code CLI.

## Usage Examples

### Basic Spec Creation

Create a simple spec and generate a plan.

```bash
/specify Build a weather dashboard that shows current conditions and 7-day forecast for user-selected cities
/plan Use React for frontend, Express.js for backend, OpenWeatherMap API for data
/tasks
```

### Advanced Research Integration

Research technologies first, then plan a complex system.

```bash
/specify Create an e-commerce platform with payment processing, inventory management, and user reviews
/plan Research Stripe integration, PostgreSQL for data, Redis for caching, Docker for deployment
```

### Constitution Compliance

Check if your spec follows simplicity rules.

```bash
/specify Build a microservice architecture with 5 services
```

The system warns about complexity and suggests fewer services.

# References

- [GitHub Spec Kit Repository](https://github.com/github/spec-kit)
- [QWEN Code CLI Repository](https://github.com/QwenLM/qwen-code)
- [Mastering Spec-Driven Development with GitHub Spec Kit](/posts/2025-09-08-mastering-spec-driven-development-with-github-spec-kit).


