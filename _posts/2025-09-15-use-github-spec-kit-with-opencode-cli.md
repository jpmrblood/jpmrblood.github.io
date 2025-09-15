---
layout: single
title: "Use GitHub Spec Kit with Opencode CLI"
tags:
   - ai
   - github
   - api
   - development
   - specifications
   - software-engineering
   - productivity
   - opencode
   - cli
categories:
   - tech
   - programming
   - ai-tools
excerpt: "Learn how to use GitHub Spec Kit with Opencode CLI for enhanced spec-driven development. This feature request integration provides advanced AI assistance for specification management."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/github-spec-kit.png
  overlay_image: /assets/2025/09/github-spec-kit.png
---

**TL;DR**:
- Install prerequisites: Opencode CLI, Astral UV, Git.
- Initialize a new Spec Kit project.
- Verify Opencode configuration.
- Use Spec Kit commands in Opencode CLI for spec-driven development.

# Step-by-Step Integration Guide

## Step 1: Initialize Spec Project

Create a new project directory with GitHub Spec Kit configured for Opencode CLI.

### Option A: New Project Directory

```bash
uvx --from git+https://github.com/aemr3/spec-kit.git specify init --ai opencode <PROJECT_NAME>
cd <PROJECT_NAME>
```

Replace `<PROJECT_NAME>` with your desired project name.

### Option B: Current Directory

Initialize Spec Kit in your current directory.

```bash
uvx --from git+https://github.com/aemr3/spec-kit.git specify init --ai opencode --here
```

## Step 2: Verify Configuration

After initialization, verify the setup.

```bash
ls -la
opencode config list
ls -la AGENTS.md
```

## Step 3: Test Integration

Launch Opencode CLI and test Spec Kit commands.

```bash
opencode
/specify Create a simple web application with user authentication
/plan Use React for frontend, Node.js for backend, PostgreSQL for database
/tasks
```

## Usage Examples

### Basic Spec Creation

Create a simple spec and generate a plan.

```bash
/specify Build a REST API for user management with authentication
/plan Use FastAPI for backend, PostgreSQL for data, JWT for auth
/tasks
```

### Advanced Research Integration

Research technologies first, then plan a complex system.

```bash
/specify Create a microservices architecture for an e-commerce platform
/plan Research Kubernetes deployment, service mesh, API gateway
```

### Constitution Compliance

Check if your spec follows simplicity rules.

```bash
/specify Build a monolithic application with 6 services
```

The system warns about complexity and suggests fewer services.

# References

- [GitHub Spec Kit Repository](https://github.com/github/spec-kit)
- [Opencode CLI Repository](https://github.com/opencode/cli)
- [Mastering Spec-Driven Development with GitHub Spec Kit](/posts/2025-09-08-mastering-spec-driven-development-with-github-spec-kit).