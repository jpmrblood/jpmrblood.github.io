---
layout: single
title: "How to Build an App with GitHub's Spec Kit: A Step-by-Step Guide"
date: 2025-09-16
tags:
  - spec-kit
  - github
  - ai
  - development
  - tutorial
  - spec-driven
categories:
  - ai
  - tools
  - development
  - tutorial
excerpt: "A comprehensive step-by-step guide to building applications using GitHub's Spec Kit toolkit for Spec-Driven Development, following along with the official video tutorial."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:

- **Spec Kit Overview**: GitHub's toolkit for Spec-Driven Development that uses AI assistants to build software from specifications.
- **Workflow**: Initialize project → Define constitution → Create spec → Generate plan → Break into tasks → Implement automatically.
- **AI Integration**: Supports multiple AI assistants (Copilot, Claude, Gemini, Cursor) for different development phases.
- **Practical Example**: Build a complete podcast website from a single natural language description.
- **Key Commands**: `/specify`, `/plan`, `/tasks`, and automated implementation execution.

This is an article created from:

<iframe width="560" height="315" src="https://www.youtube.com/embed/a9eR1xsfvHg?si=_0ju1VLGbBsGSocC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

In this tutorial, we'll follow along with Spec Kit maintainer Den Delimarsky to build a complete web application from scratch using Spec-Driven Development. This guide will break down the exact commands and workflow shown in the video.

### The Goal

We will build a modern, sleek, and responsive static website for a podcast. The site will include a landing page, an episodes page, an about page, and an FAQ page, all populated with mock data.

### Step 1: Initialize the Project

First, we need to create our project structure using the `specify` command-line tool. This tool bootstraps the entire project, setting up the necessary files and directories.

1.  Open your terminal.
2.  Run the following command, which uses `uvx` to execute the `specify` tool directly from its GitHub repository. Replace `podsite` with your desired project name.

    ```bash
    uvx --from git+https://github.com/github/spec-kit.git specify init podsite
    ```

3.  The tool will launch an interactive setup. You'll be asked to choose your preferred AI assistant. In the video, **GitHub Copilot** is selected.

4.  Next, choose your script type. Since the demonstration is on Windows, **PowerShell** is selected. The tool will automatically default to the appropriate shell for your operating system.

The tool will then create a new directory named `podsite` and populate it with the initial project files.

### Step 2: Set Up the Workspace

Now that the project is initialized, let's navigate into the directory and open it in a code editor like VS Code.

1.  In your terminal, change into the new project directory:

    ```bash
    cd podsite
    ```

2.  Open the folder in VS Code:

    ```bash
    code .
    ```

You will see the generated file structure, which includes a `.github` folder with prompts for the AI and a `.specify` folder containing templates and scripts that orchestrate the development process.

### Step 3: Define the Project's Constitution

The `constitution.md` file is where you lay down the core, non-negotiable principles for your project. This guides the AI in making decisions that align with your high-level goals.

1.  Open the `memory/constitution.md` file.
2.  Using the GitHub Copilot Chat (or your chosen AI assistant's interface), provide a prompt to populate this file.

    **Prompt used in the video:**
    > Fill the constitution with the bare minimum requirements for a static web app based on the template.

3.  The AI will generate a set of core principles, such as Static-First Delivery, Simplicity Over Tooling, and Accessibility & SEO Baselines. Review and accept these changes.

### Step 4: Create the Feature Specification

This is where you describe *what* you want to build in plain English.

1.  In the Copilot Chat panel, use the `/specify` command. This command takes your natural language description and creates a formal specification document.

    **Prompt used in the video:**
    > /specify I am building a modern podcast website. I want it to look sleek, something that would stand out. Should have a landing page with one featured episode. There should be an episodes page, an about page, and a FAQ page. Should have 20 episodes, and the data is mocked - you do not need to pull anything from any real feed.

2.  After you run the command, Spec Kit will:
    *   Create a new Git branch for the feature (e.g., `001-i-am-building`).
    *   Generate a new `spec.md` file inside a `specs/` directory.

This new `spec.md` file is a structured document containing user stories, acceptance scenarios, and functional requirements based on your prompt. It will also include `[NEEDS CLARIFICATION]` tags where the AI identified ambiguities.

### Step 5: Generate the Implementation Plan

With a clear specification, the next step is to create a technical plan for *how* to build it.

1.  In Copilot Chat, use the `/plan` command, providing the technical stack and architectural choices.

    **Prompt used in the video:**
    > /plan I am going to use Next.js with static site configuration, no databases - data is embedded in the content for the mock episodes. Site is responsive and ready for mobile.

2.  Spec Kit will now generate a complete implementation plan. This includes creating several new markdown files in your spec folder, such as:
    *   `plan.md`: The high-level technical plan.
    *   `data-model.md`: A description of the data structures.
    *   `quickstart.md`: Instructions for setting up and running the project.
    *   `research.md`: Research notes and decisions made by the AI.

### Step 6: Break the Plan into Actionable Tasks

Now, we'll break down the high-level plan into a detailed, step-by-step checklist for the AI to follow.

1.  In Copilot Chat, use the `/tasks` command.

    **Prompt used in the video:**
    > /tasks break this down into tasks

2.  This command generates a `tasks.md` file. This file contains a comprehensive list of all the steps needed to build the application, from setting up the Next.js skeleton and installing dependencies to creating components, pages, and tests.

### Step 7: Execute the Implementation

This is where the magic happens. We will now instruct the AI to execute the tasks it just created.

1.  In Copilot Chat, give the final command to start the implementation.

    **Prompt used in the video:**
    > Implement the tasks for this project, and update the task list as you go.

2.  The AI will now begin working through the `tasks.md` file. It will:
    *   Run terminal commands to install packages.
    *   Create and modify source code files (.ts, .tsx, .css, etc.).
    *   Write tests and ensure they pass.
    *   Update the checkboxes in `tasks.md` as it completes each step.

You can watch in real-time as your application is built, file by file. The entire process, from a single sentence to a running application, is automated.

### Step 8: Run Your New Application

Once the AI has completed all the tasks, your application is ready.

1.  The AI will likely have already started the development server, but if not, you can run it yourself from the terminal:
    ```bash
    npm run dev
    ```
2.  Open your browser and navigate to `http://localhost:3000`.

You will see the completed podcast website, built entirely from your initial specification, all thanks to the structured workflow of the Spec Kit. From here, you can continue to add new features by repeating the process: specify, plan, task, and implement.

## References

- [GitHub Spec Kit Repository](https://github.com/github/spec-kit)
- [Spec Kit Video Tutorial](https://www.youtube.com/watch?v=a9eR1xsfvHg)
- [Spec-Driven Development Methodology](https://github.com/github/spec-kit/blob/main/spec-driven.md)
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [uv Package Manager](https://docs.astral.sh/uv/)