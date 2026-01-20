---
layout: single
title: "OpenSpec Tutorial: Spec-Driven Development for AI Coding Assistants"
excerpt: "Learn how to use OpenSpec to align humans and AI coding assistants with spec-driven development. No API keys required - works with Claude Code, Cursor, and other AI tools."
categories: [tools, tutorial, ai-development]
tags: [ai-coding, spec-driven-development, openspec, claude-code, cursor, prompt-engineering]
header:
  overlay_image: /assets/2025/10/openspec-dashboard.png
  overlay_filter: rgba(0, 0, 0, 0.7)
---
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10.6.1/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>

## TL;DR

- **OpenSpec** enables spec-driven development for AI coding assistants
- **No API keys required** - works with existing AI tools you already use
- **Change tracking** through structured folders (proposals, tasks, spec updates)
- **Brownfield-first** approach works great for modifying existing codebases
- **Native integration** with Claude Code, Cursor, OpenCode, and other tools

## Why OpenSpec? The Problem with AI Coding Today

AI coding assistants are incredibly powerful, but they have a fundamental problem: **unpredictability**. When requirements live only in chat history, you get:

- Misunderstood requirements leading to wrong implementations
- Inconsistent outputs across different AI sessions
- Difficulty tracking what was agreed upon vs. what was implemented
- "AI bug loops" where you iterate endlessly on misunderstood features

OpenSpec solves this by adding a **lightweight specification workflow** that locks intent before implementation begins.

## What Makes OpenSpec Different

### Key Advantages

1. **Lightweight**: Simple workflow, no API keys, minimal setup
2. **Brownfield-first**: Excels at modifying existing behavior (1→n), not just 0→1
3. **Change tracking**: Proposals, tasks, and spec deltas live together
4. **Tool agnostic**: Works with AI tools you already use

### How It Compares

| Tool | Focus | OpenSpec Advantage |
|------|-------|-------------------|
| **spec-kit** | Brand-new features (0→1) | Better for cross-spec updates and evolving features |
| **Kiro.dev** | Individual spec management | Groups related changes together in feature folders |
| **No specs** | Chat-based development | Predictable, reviewable outputs with clear intent |

## Installation and Setup

### Prerequisites
- **Node.js >= 20.19.0**
Make a new tutorial based on the Openspec: https://github.com/Fission-AI/OpenSpec 
### Step 1: Install OpenSpec CLI
```bash
npm install -g @fission-ai/openspec@latest
```

Verify installation:
```bash
openspec --version
```

### Step 2: Initialize in Your Project
```bash
cd my-project
openspec init
```

**What happens during initialization:**
- Prompts you to select supported AI tools (Claude Code, Cursor, OpenCode, etc.)
- Creates `openspec/` directory structure
- Configures slash commands for selected tools
- Generates `AGENTS.md` file for AI guidance

## OpenSpec Project Structure

After initialization, your project gets this structure:

```
openspec/
├── specs/           # Current source of truth
│   └── auth/
│       └── spec.md
└── changes/         # Proposed updates
    └── add-2fa/
        ├── proposal.md      # Why and what changes
        ├── tasks.md         # Implementation checklist
        ├── design.md        # Technical decisions (optional)
        └── specs/
            └── auth/
                └── spec.md  # Delta showing additions
```

## The OpenSpec Workflow

Here's a visual representation of how OpenSpec works:

<div class="mermaid">
graph TD
    A[Requirements Discussion] --> B[Draft Change Proposal]
    B --> C[Review & Refine Specs]
    C --> D[Validate & Approve Specs]
    D --> E[Implement Tasks]
    E --> F[Archive Change]

    B -.-> G[openspec/changes/feature-name/]
    G -.-> H[proposal.md]
    G -.-> I[tasks.md]
    G -.-> J[specs/spec.md delta]

    E -.-> K[openspec/specs/ updated]
    F -.-> L[Change merged to specs]

    style A fill:#e1f5fe
    style F fill:#c8e6c9
    style G fill:#fff3e0
    style K fill:#fff3e0
</div>

### Project Structure Diagram

<div class="mermaid">
graph TB
    A[your-project/] --> B[openspec/]
    A --> C[src/]

    B --> D[specs/]
    B --> E[changes/]
    B --> F[archive/]

    D --> G[auth/spec.md]
    D --> H[profiles/spec.md]

    E --> I[add-user-profiles/]
    E --> J[enhance-search/]

    I --> K[proposal.md]
    I --> L[tasks.md]
    I --> M[design.md]
    I --> N[specs/profiles/spec.md]

    J --> O[proposal.md]
    J --> P[tasks.md]
    J --> Q[specs/search/spec.md]

    F --> R[add-2fa/]
    R --> S[proposal.md]
    R --> T[tasks.md]
    R --> U[specs/auth/spec.md]

    style D fill:#e8f5e8
    style E fill:#fff3e0
    style F fill:#fce4ec
</div>

### 1. Draft a Change Proposal

Start by asking your AI to create a change proposal:

```text
You: Create an OpenSpec change proposal for adding profile search filters by role and team
     (Shortcut for tools with slash commands: /openspec:proposal Add profile search filters)

AI:  I'll create an OpenSpec change proposal for profile filters.
     *Creates openspec/changes/add-profile-filters/ with proposal.md, tasks.md, spec deltas.*
```

### 2. Review and Validate

Check that everything looks correct:

```bash
openspec list                    # View active changes
openspec validate add-profile-filters    # Check spec formatting
openspec show add-profile-filters        # Review proposal, tasks, and spec delta
```

### 3. Refine the Specifications

Iterate until the specs match your needs:

```text
You: Can you add acceptance criteria for the role and team filters?

AI:  I'll update the spec delta with scenarios for role and team filters.
     *Edits openspec/changes/add-profile-filters/specs/profile/spec.md and tasks.md.*
```

### 4. Implement the Change

Once specs are approved, implement:

```text
You: The specs look good. Let's implement this change.
     (Shortcut: /openspec:apply add-profile-filters)

AI:  I'll work through the tasks in the add-profile-filters change.
     *Implements tasks from openspec/changes/add-profile-filters/tasks.md*
```

### 5. Archive the Completed Change

After implementation, archive to merge changes back into specs:

```text
You: Please archive the change
     (Shortcut: /openspec:archive add-profile-filters)

AI:  I'll archive the add-profile-filters change.
     ✓ Change archived successfully. Specs updated.
```

## Native Integration with AI Tools

### Tools with Built-in Slash Commands

| Tool | Commands |
|------|----------|
| **Claude Code** | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` |
| **Cursor** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` |
| **OpenCode** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` |
| **Kilo Code** | `/openspec-proposal.md`, `/openspec-apply.md`, `/openspec-archive.md` |
| **Windsurf** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` |
| **Codex** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` |

### AGENTS.md Compatible Tools

Tools that read `openspec/AGENTS.md` automatically:
- Amp • Jules • Gemini CLI • Other AGENTS.md compatible tools

## Writing Effective Specifications

### Spec Format Requirements

OpenSpec uses a structured format for specifications:

```markdown
# Auth Specification

## Purpose
Authentication and session management.

## Requirements
### Requirement: User Authentication
The system SHALL issue a JWT on successful login.

#### Scenario: Valid credentials
- WHEN a user submits valid credentials
- THEN a JWT is returned
```

### Delta Format for Changes

When proposing changes, use this format:

```markdown
# Delta for Auth

## ADDED Requirements
### Requirement: Two-Factor Authentication
The system MUST require a second factor during login.

#### Scenario: OTP required
- WHEN a user submits valid credentials
- THEN an OTP challenge is required

## MODIFIED Requirements
### Requirement: User Authentication
The system SHALL issue a JWT on successful login AND require OTP verification.

## REMOVED Requirements
### Requirement: Password-only Authentication
[Deprecated - replaced by 2FA requirement above]
```

## Command Reference

### Core Commands
```bash
openspec list              # View active change folders
openspec view              # Interactive dashboard of specs and changes
openspec show <change>     # Display change details
openspec validate <change> # Check spec formatting
openspec archive <change>  # Archive completed change
```

### Advanced Commands
```bash
openspec init              # Initialize OpenSpec in project
openspec update            # Refresh AI tool integrations
openspec archive <change> --yes  # Archive without prompts
```

## Real-World Example: Adding User Profiles

Let's walk through a complete example of adding user profile functionality:

### 1. Create the Proposal
```text
You: Create an OpenSpec change proposal for adding user profile management with avatar upload

AI:  I'll create a change proposal for user profile management.
     *Creates openspec/changes/add-user-profiles/*
```

### 2. Review Generated Files

The AI creates these files:

**`openspec/changes/add-user-profiles/proposal.md`:**
```markdown
# Add User Profile Management

## Purpose
Allow users to manage their profile information and upload avatar images.

## Goals
- Users can view and edit their profile information
- Users can upload and change their avatar image
- Profile changes are persisted to the database
```

**`openspec/changes/add-user-profiles/tasks.md`:**
```markdown
## 1. Database Schema
- [ ] 1.1 Add profile fields to users table (bio, location, website)
- [ ] 1.2 Add avatar_url column to users table

## 2. Backend API
- [ ] 2.1 Create profile update endpoint
- [ ] 2.2 Create avatar upload endpoint
- [ ] 2.3 Add profile validation

## 3. Frontend Components
- [ ] 3.1 Create profile edit form
- [ ] 3.2 Create avatar upload component
- [ ] 3.3 Add profile view to user menu
```

**`openspec/changes/add-user-profiles/specs/profiles/spec.md`:**
```markdown
# User Profile Specification

## Purpose
User profile management and avatar handling.

## Requirements
### Requirement: Profile Information
Users MUST be able to view and edit their profile information.

#### Scenario: Edit profile
- WHEN a user navigates to their profile
- THEN they can edit bio, location, and website fields
- AND changes are saved to the database

### Requirement: Avatar Upload
Users MUST be able to upload and change their avatar image.

#### Scenario: Upload avatar
- WHEN a user selects an image file
- THEN the image is uploaded to cloud storage
- AND the avatar_url is updated in the database
```

### 3. Implementation Phase

Once you approve the specs, the AI implements each task:

```text
You: The profile specs look good. Let's implement this.
     (or use /openspec:apply add-user-profiles)

AI:  I'll implement the user profile management feature.
     *Implements database schema changes*
     *Creates backend API endpoints*
     *Builds frontend components*
     *Updates routing and navigation*
```

### 4. Archive and Update

After completion:

```text
You: Archive the completed profile change
     (or use /openspec:archive add-user-profiles)

AI:  Archiving add-user-profiles change...
     ✓ Profile specs merged into main specification
     ✓ Change moved to archive folder
```

## Best Practices for OpenSpec

### 1. Start Small
- Begin with one feature at a time
- Use OpenSpec for new features or significant changes
- Don't try to spec your entire codebase at once

### 2. Be Specific in Specs
- Use "SHALL" and "MUST" in requirement statements
- Include concrete scenarios for each requirement
- Specify both happy path and edge cases

### 3. Regular Maintenance
- Run `openspec update` when switching AI tools
- Archive completed changes regularly
- Keep specs up to date with actual implementation

### 4. Team Collaboration
- Share OpenSpec folders in code reviews
- Use change proposals for feature discussions
- Archive changes only after team approval

## Troubleshooting Common Issues

### Problem: AI Doesn't Follow OpenSpec Workflow
**Solution**: Ensure `AGENTS.md` exists and is up to date
```bash
openspec update  # Regenerates AI guidance
```

### Problem: Spec Validation Errors
**Solution**: Check formatting requirements:
- Use proper heading hierarchy (`#`, `##`, `###`, `####`)
- Include scenarios for each requirement
- Use SHALL/MUST language in requirements

### Problem: Tool Doesn't Recognize Slash Commands
**Solution**: Verify tool selection during `openspec init`
- Re-run `openspec init` to add missing tools
- Check that slash command files were created correctly

## Advanced OpenSpec Patterns

### Multi-Spec Changes
For features that span multiple domains:

```
openspec/changes/add-user-messaging/
├── proposal.md
├── tasks.md
└── specs/
    ├── profiles/
    │   └── spec.md      # Profile privacy settings
    ├── messaging/
    │   └── spec.md      # Message system
    └── notifications/
        └── spec.md      # Message notifications
```

### Iterative Development
For complex features, break into phases:

```
openspec/changes/enhance-search-phase-1/  # Basic filters
openspec/changes/enhance-search-phase-2/  # Advanced filters
openspec/changes/enhance-search-phase-3/  # AI-powered suggestions
```

## Conclusion

OpenSpec transforms AI-assisted development from unpredictable chat-based coding to structured, spec-driven development. By agreeing on specifications before implementation begins, you eliminate ambiguity and create a clear trail of decisions and changes.

The tool's brownfield-first approach makes it perfect for existing projects, while its lightweight nature ensures it won't slow down your development process. Whether you're using Claude Code, Cursor, or any other AI coding assistant, OpenSpec provides the structure needed for reliable, predictable AI-assisted development.

## Resources

- **OpenSpec Repository**: [github.com/Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)
- **Discord Community**: [Join OpenSpec Discord](https://discord.gg/YctCnvvshC)
- **Twitter Updates**: [@0xTab on X](https://x.com/0xTab)
- **npm Package**: [@fission-ai/openspec](https://www.npmjs.com/package/@fission-ai/openspec)

## References

1. **Fission AI**. (2024). *OpenSpec: Spec-driven development for AI coding assistants*. GitHub Repository. https://github.com/Fission-AI/OpenSpec

2. **Tabish**. (2024). *OpenSpec Documentation and Examples*. https://github.com/Fission-AI/OpenSpec/blob/main/README.md

---

*This tutorial covers OpenSpec v0.9.1 and is based on the official documentation and examples provided by the Fission AI team.*
