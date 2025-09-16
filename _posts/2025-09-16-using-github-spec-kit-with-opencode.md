---
layout: single
title: "Using GitHub Spec Kit with OpenCode: A Comprehensive Guide"
date: 2025-09-16
tags:
  - spec-kit
  - github
  - opencode
  - ai
  - development
  - integration
categories:
  - ai
  - tools
  - development
excerpt: "Learn how to integrate GitHub Spec Kit with OpenCode for enhanced spec-driven development workflows, combining AI assistance with powerful coding tools."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:

- **Integration Overview**: Combining GitHub Spec Kit's spec-driven development with OpenCode's AI-powered coding capabilities.
- **Workflow Enhancement**: Streamlined process for building applications using specifications and advanced AI assistance.
- **Tool Synergy**: Leveraging both Spec Kit's structured approach and OpenCode's intelligent code generation.
- **Practical Implementation**: Step-by-step guide to setting up and using the integrated workflow.

## Introduction

This guide builds on the [general Spec Kit tutorial](**/posts**/2025-09-16-howto-github-spec-kit-guide) by focusing on integrating GitHub Spec Kit with OpenCode, an advanced AI coding assistant. While the general guide uses GitHub Copilot, this tutorial shows how to leverage OpenCode's enhanced capabilities for spec-driven development.

I already tried to install [opencode integration from a PR.](**/posts**/2025-09-15-use-github-spec-kit-with-opencode-cli). From further reading, I found out that Opencode uses the same format as Claude with additional parameters for choosing agent and model.

## Prerequisites

- OpenCode CLI installed and configured
- GitHub Spec Kit repository cloned or accessible via `uvx`
- Basic knowledge of spec-driven development
- VSCode or compatible editor with OpenCode extension

## Step 1: Initialize the Project

First, create your project structure using the `specify` command-line tool with OpenCode integration.

1. Open your terminal.
2. Run the following command to initialize with Claude template (which OpenCode will adapt):

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --ai claude --script sh --ignore-agent-tools --here
```

3. The tool will create the necessary files and directories for spec-driven development.

## Step 2: Set Up OpenCode Integration

Now, configure OpenCode to work with the Spec Kit files. Open the project in VSCode.In the OpenCode chat panel, prompt:

```text
This is a spec-driven project created using Claude. The goal is to change any reference to claude code to you (opencode).

1. Move `.claude/commands` folder to `.opencode/command`
2. Remove the empty .claude folder
3. Search in .opencode and .specify directories for any reference to “claude”, see the pattern and add “opencode” also.
4. Exception for file used for memory: CLAUDE.md → AGENTS.md
```

NOTE: Special for this prompt, I use code block instead of quote block. The usage of backtick is important. I need to test this over and over so the prompt is always conistent. Using quote or single quote will make the LLM **(Grok Code Fast I)** put the trailing `s`. It will copy `.claude/commands` --> `.opencode/commands`.

OpenCode will modify the `.claude` and `.specify` files to be compatible with its format. The command also will not obstrusive and retains the Claude Code references.

## Step 3: Define the Project's Constitution

The `constitution.md` file sets core principles for your project.

1. Open the `.specify/memory/constitution.md` file.
2. Using OpenCode, provide a prompt to populate this file:

> Fill the constitution with the bare minimum requirements for a static web app based on the template.

Or, your project type.

OpenCode will generate non-negotiable core principles like Static-First Delivery, Simplicity Over Tooling, and Accessibility & SEO Baselines.

## Step 4: Create the Feature Specification

Describe what you want to build in plain English.

1. In OpenCode chat, use the `**/specify**` command:

> **/specify** I am building a modern podcast website. I want it to look sleek, something that would stand out. Should have a landing page with one featured episode. There should be an episodes page, an about page, and a FAQ page. Should have 20 episodes, and the data is mocked.

2. OpenCode will create a `spec.md` file with user stories, acceptance scenarios, and functional requirements.

## Step 5: Generate the Implementation Plan

Create a technical plan for how to build it.

1. In OpenCode chat, use the `**/plan**` command:

> **/plan** I am going to use Next.js with static site configuration, no databases - data is embedded in the content for the mock episodes. Site is responsive and ready for mobile.

2. OpenCode will generate plan files including `plan.md`, `data-model.md`, `quickstart.md`, and `research.md`.

## Step 6: Break the Plan into Actionable Tasks

Break down the plan into detailed steps.

1. In OpenCode chat, use the `**/tasks**` command:

> **/tasks** break this down into tasks

2. This generates a `tasks.md` file with a comprehensive checklist of implementation steps.

## Step 7: Execute the Implementation

Instruct OpenCode to implement the tasks.

1. In OpenCode chat, give the final command:

> Implement the tasks for this project, and update the task list as you go.

2. OpenCode will work through the `tasks.md` file, creating code files, running commands, and updating progress.

## Step 8: Run Your New Application

Once implementation is complete:

1. Run the development server:

```bash
bun run dev
```

2. Open your browser to `http//localhost:3000` to see the completed application.

## Advanced Features

- **Multi-Agent Support**: Specify different agents for various tasks (e.g., research, coding, testing)
- **Model Selection**: Choose specific AI models for different complexity levels
- **Constitution Compliance**: Automatic checks for spec simplicity and feasibility
- **Integration with Git**: Version control for specs and generated code

## Best Practices

- Start with simple, focused specs before scaling complexity
- Regularly review and update specs as requirements evolve
- Use version control for both code and spec files
- Combine human oversight with AI assistance for optimal results
- Test generated code thoroughly before deployment

## Troubleshooting

- **Spec Not Generating**: Ensure OpenCode is properly configured and has access to the project directory
- **Model Selection Issues**: Verify agent parameters are correctly set in the modified files
- **Integration Errors**: Check that `.claude` and `.specify` files were properly updated by OpenCode
- **Performance Problems**: Reduce spec complexity or switch to lighter AI models

## Conclusion

Integrating GitHub Spec Kit with OpenCode creates a powerful workflow for spec-driven development, combining structured specification management with advanced AI coding assistance. This approach enhances productivity while maintaining code quality and project clarity. Start with simple projects to master the workflow, then scale to more complex applications.

## References

- [GitHub Spec Kit Repository](https://github.com/github/spec-kit)
- [OpenCode Documentation](https://opencode.ai)
- [Spec-Driven Development Methodology](https://github.com/github/spec-kit/blob/main/spec-driven.md)