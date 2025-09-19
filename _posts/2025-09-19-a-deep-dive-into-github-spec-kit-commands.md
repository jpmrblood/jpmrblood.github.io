---
layout: single
title: "A Guide to GitHub Spec Kit Commands and MCP Integration"
date: 2025-09-19
tags:
  - spec-kit
  - github
  - gemini-cli
  - ai
  - development
  - workflow
  - mcp
categories:
  - ai
  - tools
  - development
excerpt: "A comprehensive guide to using the core commands of GitHub Spec Kit, and how to integrate them with Memory and Documentation MCPs for an enhanced, AI-driven workflow."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:

- **Core Commands**: Understand the purpose of the 5 main Spec Kit commands: `constitution`, `specify`, `plan`, `tasks`, and `implement`.
- **MCP Integration**: Learn how to manually use Memory (save/recall facts) and Documentation (fetch latest docs) services to guide the workflow.
- **Workflow Enhancement**: Weave MCP commands between Spec Kit commands to make better, data-driven decisions during development.
- **Advanced Automation**: Modify the internal prompts of Spec Kit commands to make them "smarter" and automatically consult MCPs during their execution.

## Introduction

This guide provides a deep dive into the commands used in the GitHub Spec Kit, a powerful tool for spec-driven development. We will cover the core commands that form the backbone of the workflow and then explore how to supercharge this process by integrating two powerful external services (MCPs): a Memory service for persistent knowledge and a Documentation service for accessing up-to-date library information.

## The Core Spec Kit Commands

These five commands represent the key stages of the spec-driven development lifecycle.

### 1. `constitution`
The `constitution` command establishes the foundational principles of your project. It creates and manages the `constitution.md` file, which acts as a set of non-negotiable rules and guidelines that the AI must follow.

> **Use Case:** At the start of a new project, define its core tenets (e.g., "All endpoints must be secured," "Code must be fully tested").
>
> **Example Prompt:**
> > /constitution "Project: My Awesome App. Principles: 1. All code must be peer-reviewed. 2. All features must have unit tests. 3. Use semantic versioning."

### 2. `specify`
This command takes a natural language description of a feature and converts it into a detailed `spec.md` file, complete with user stories, functional requirements, and acceptance criteria.

> **Use Case:** A product manager wants to formalize a feature request into a structured document for the development team.
>
> **Example Prompt:**
> > /specify "Create a user login page with email and password fields. Include a 'Forgot Password' link. After login, redirect the user to their dashboard."

### 3. `plan`
The `plan` command takes the `spec.md` file and generates a comprehensive technical plan. This includes proposing a tech stack, defining data models, outlining API contracts, and creating other essential design artifacts.

> **Use Case:** A developer needs to create a technical blueprint for how to build the feature defined in the specification.
>
> **Example Prompt:**
> > /plan "feature: user-login. Tech stack: React frontend, Node.js backend with Express, PostgreSQL database. Authentication with JWT."

### 4. `tasks`
This command breaks down the high-level technical plan into a detailed, actionable checklist of development tasks in a `tasks.md` file. It intelligently determines dependencies and identifies tasks that can be worked on in parallel.

> **Use Case:** A team lead needs to convert the technical plan into a list of concrete tasks for developers to execute.
>
> **Example Prompt:**
> > /tasks "feature: user-login"

### 5. `implement`
The `implement` command is the final execution step. The AI works through the `tasks.md` file, writing code, running commands, and marking tasks as complete until the feature is fully implemented.

> **Use Case:** A developer is ready to write the code and wants the AI to execute the pre-defined task list.
>
> **Example Prompt:**
> > /implement "feature: user-login"

## Integrating External Services (MCPs)

You can significantly enhance the Spec Kit workflow by manually weaving in calls to the Memory and Documentation MCPs.

### Accessing Up-to-Date Documentation
Use this when you need to research a library during the `plan` or `implement` stage.

1.  **Find the Library ID:**
    ```bash
    /resolve_library_id "Next.js"
    ```
2.  **Get the Docs:**
    ```bash
    /get_library_docs "/vercel/next.js" --topic "routing"
    ```

### Creating a Persistent Learning Experience
Use this at any stage to save and recall important decisions or facts.

1.  **Store a Fact:**
    ```bash
    /store_memory "The project requires all API endpoints to be authenticated using JWT." --metadata '{"tags": ["project-requirements", "authentication"]}'
    ```
2.  **Recall a Fact:**
    ```bash
    /retrieve_memory "API authentication requirements"
    ```

## Advanced: Making Commands Smarter with MCP Integration

For a truly automated workflow, you can modify a command's internal prompt to force it to consult the MCPs.

### Example: Upgrading the `/plan` Command
**Goal:** Force the `/plan` command to automatically research proposed technologies and save its decisions.

1.  **Locate the Command File:** Find the `plan.toml` file in your Spec Kit's `.gemini/commands/` directory.
2.  **Replace the Prompt:** Edit the file and replace the entire `prompt = """..."""` block with the enhanced version below, which adds a "Technology Research and Validation" step.

```toml
prompt = """
---
description: Execute the implementation planning workflow using the plan template to generate design artifacts.
---

Given the implementation details provided as an argument, do this:

1. Run `.specify/scripts/bash/setup-plan.sh --json` from the repo root and parse JSON for FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. All future file paths must be absolute.
2. Read and analyze the feature specification to understand:
   - The feature requirements and user stories
   - Functional and non-functional requirements
   - Success criteria and acceptance criteria
   - Any technical constraints or dependencies mentioned

3. Read the constitution at `.specify/memory/constitution.md` to understand constitutional requirements.

4. **Technology Research and Validation (MCP Integration)**
   - For each key technology or library proposed in the feature description or technical context ({{args}}):
     a. **Consult Memory:** You MUST first use the `retrieve_memory` tool with the technology name and relevant keywords (e.g., "database", "frontend framework") to check for existing decisions, patterns, or code snippets.
     b. **Consult Documentation:** You MUST then use `resolve_library_id` to get the library's ID, followed by `get_library_docs` to fetch current documentation. Focus on topics directly related to the feature's requirements (e.g., for a real-time feature, search for "websockets", "real-time", "streaming").
     c. **Synthesize and Decide:** Based on the retrieved memory and fresh documentation, formulate a definitive choice for the technology. State the rationale for your decision clearly.
     d. **Record Decision:** You MUST use the `store_memory` tool to save this decision and its rationale. Use tags such as "tech-stack", "decision", and the feature name for future retrieval.

5. Execute the implementation plan template:
   - Load `.specify/templates/plan-template.md` (already copied to IMPL_PLAN path)
   - **Crucially, inject the findings and decisions from the research step (4) into the 'Technical Context' or 'Architecture' sections of the plan.**
   - Set Input path to FEATURE_SPEC
   - Run the Execution Flow (main) function steps 1-9
   - The template is self-contained and executable
   - Follow error handling and gate checks as specified
   - Let the template guide artifact generation in $SPECS_DIR:
     * Phase 0 generates research.md
     * Phase 1 generates data-model.md, contracts/, quickstart.md
     * Phase 2 generates tasks.md
   - Incorporate user-provided details from arguments into Technical Context: {{args}}
   - Update Progress Tracking as you complete each phase

6. Verify execution completed:
   - Check Progress Tracking shows all phases complete
   - Ensure all required artifacts were generated
   - Confirm no ERROR states in execution

7. Report results with branch name, file paths, and generated artifacts.

Use absolute paths with the repository root for all file operations to avoid path issues.
"""
```

## Best Practices
- **Start Simple:** Begin with small, well-defined features to get comfortable with the workflow.
- **Tag Memories:** Use descriptive tags when using `store_memory` for easier retrieval later.
- **Review AI Output:** Always review the specs, plans, and code generated by the AI. Human oversight is key.
- **Iterate:** Don't expect a perfect result on the first try. Refine your specs and plans as you go.

## Troubleshooting
- **Permission Errors:** If you get errors when modifying command files, you may need to copy them into the workspace, edit them, and then copy them back.
- **Inaccurate Output:** If a command produces a poor result, try making your input prompt more specific. For `/plan` or `/tasks`, ensure the preceding `spec.md` is clear and detailed.
- **MCP Failures:** Ensure your MCP services are running and accessible from the Gemini CLI environment.

## Conclusion

The GitHub Spec Kit provides a powerful, structured approach to AI-driven development. By understanding its core commands and integrating them with powerful external services for memory and documentation, you can create a highly efficient, intelligent, and automated development workflow.

## References

- GitHub Spec Kit Repository (Link to be added)
- Memory and Documentation Service Docs (Link to be added)
