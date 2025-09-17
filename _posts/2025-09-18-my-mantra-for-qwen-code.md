---
layout: single
title: "My Mantra for QWEN Code"
tags:
   - ai
   - github
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
excerpt: "Learn my approach to using Qwen Code with GitHub Spec Kit for spec-driven development. Discover workflows that combine specification management with AI-powered coding assistance for more efficient software projects."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/09/github-spec-kit.png
  overlay_image: /assets/2025/09/github-spec-kit.png
---

**TL;DR**:
- Initialize GitHub Spec Kit with Qwen Code CLI
- Use automated formatting for conventional commits
- Adapt spec files for Qwen Code compatibility
- Maintain spec-driven development workflow

# My Workflow Integration

## Step 1: Initialize Spec Project

I start by initializing a GitHub Spec Kit project in my current directory, configured for Qwen Code CLI.

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --ai gemini --script sh --here
```

## Step 2: Configure for Conventional Commits

I immediately set up automated formatting for conventional commits to maintain consistent commit history.

```bash
qwen -y -p "reformat all commits with rules from https://www.conventionalcommits.org/en/v1.0.0/#specification"
```

## Step 3: Configure QWEN Integration

After initialization, I rename the `.gemini` directory to `.qwen` for QWEN CLI compatibility.

```bash
mv .gemini .qwen
```

Then I update any spec files that reference Gemini to work with Qwen Code:

```bash
qwen -y -p "find files in .specify that contains Gemini-related usage. Copy the syntax and adapt it for Qwen Code. Strictly edit the files, add the syntax, and don't do anything else"
```

## Step 4: Commit Changes

Finally, I commit the changes with proper conventional commit formatting:

```bash
qwen -y -p "commit the changes. Format this commit and going forward with rules from https://www.conventionalcommits.org/en/v1.0.0/#specification"
```

# Benefits of This Approach

This workflow provides several advantages:

1. **Consistency**: All commits follow the conventional commits specification
2. **Automation**: Qwen Code handles the formatting and adaptation automatically
3. **Compatibility**: Properly configured for Qwen Code CLI usage
4. **Specification-Driven**: Maintains the benefits of spec-driven development

By following this mantra, I ensure that all my projects start with a consistent, well-configured foundation that leverages both GitHub Spec Kit and Qwen Code CLI effectively.
