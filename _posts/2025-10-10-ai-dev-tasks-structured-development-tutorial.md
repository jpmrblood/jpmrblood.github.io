---
layout: single
title: "AI Dev Tasks Tutorial: Structured Feature Development with AI Assistants"
excerpt: "Learn how to use AI Dev Tasks to transform unpredictable AI coding into structured, reliable feature development. Works with Cursor, Claude Code, and other AI tools."
categories: [tools, tutorial, ai-development]
tags: [ai-coding, structured-development, ai-dev-tasks, cursor, claude-code, feature-development]
header:
  overlay_image: /assets/2025/10/ai-dev-tasks-workflow.png
  overlay_filter: rgba(0, 0, 0, 0.7)
---

## TL;DR

- **AI Dev Tasks** provides a 3-step workflow for systematic AI-assisted feature development
- **Works with any AI tool**: Cursor, Claude Code, Windsurf, and other AI coding assistants
- **Structured approach**: PRD → Task List → Implementation with built-in checkpoints
- **Quality assurance**: Step-by-step verification prevents AI "black box" issues

## Why AI Dev Tasks? The Problem with Traditional AI Coding

Building complex features with AI can feel like a black box. You give a vague prompt and hope for the best, but often get:

- Misunderstood requirements leading to wrong implementations
- Inconsistent outputs across different AI sessions
- Difficulty tracking progress and debugging issues
- "AI spaghetti" where features grow uncontrollably

AI Dev Tasks solves this by providing **structured workflows** that guide your AI collaborator step-by-step.

## The 3-File System

AI Dev Tasks consists of three specialized markdown files:

| File | Purpose | When to Use |
|------|---------|-------------|
| **`create-prd.md`** | Generate Product Requirement Documents | Feature planning phase |
| **`generate-tasks.md`** | Break PRDs into actionable tasks | Implementation planning |
| **`process-task-list.md`** | Implement tasks systematically | Development phase |

## Complete Workflow: From Idea to Implementation

### Step 1: Create a Product Requirement Document (PRD)

**Goal**: Transform vague feature ideas into clear, actionable specifications.

#### How to Use `create-prd.md`

1. **Reference the file** in your AI assistant:
   ```text
   Use @create-prd.md
   Here's the feature I want to build: Add user profile editing with avatar upload
   ```

2. **Answer clarifying questions** from the AI:
   - What problem does this solve?
   - Who are the target users?
   - What are the core user actions?
   - What are the acceptance criteria?

3. **Review the generated PRD** saved as `[n]-prd-[feature-name].md`

#### Example PRD Structure

```markdown
# 0001-PRD-User Profile Editing

## Introduction/Overview
Allow users to edit their profile information and upload avatar images to personalize their accounts.

## Goals
- Users can view and edit their profile information
- Users can upload and change their avatar image
- Profile changes are persisted to the database

## User Stories
- As a user, I want to edit my profile so I can keep my information current
- As a user, I want to upload an avatar so I can personalize my profile

## Functional Requirements
1. The system must display current user profile information
2. The system must allow editing of profile fields (name, bio, location)
3. The system must allow avatar image upload
4. The system must validate image file types and sizes
```

### Step 2: Generate Your Task List

**Goal**: Break the PRD into granular, actionable development tasks.

#### How to Use `generate-tasks.md`

1. **Reference both files**:
   ```text
   Take @0001-prd-user-profile-editing.md and create tasks using @generate-tasks.md
   ```

2. **Review parent tasks** first, then approve sub-task generation:
   ```
   AI: I have generated the high-level tasks. Ready to generate sub-tasks? Respond with 'Go'
   You: Go
   ```

3. **Review the complete task list** saved as `tasks-0001-prd-user-profile-editing.md`

#### Example Task List Structure

```markdown
## Relevant Files
- `src/components/ProfileEdit.tsx` - Main profile editing component
- `src/components/ProfileEdit.test.tsx` - Unit tests for profile editing
- `src/hooks/useProfile.ts` - Custom hook for profile management
- `src/services/profileService.ts` - API service for profile operations

## Tasks

- [ ] 1.0 Set up profile editing infrastructure
  - [ ] 1.1 Create ProfileEdit component with form layout
  - [ ] 1.2 Add form validation for required fields
  - [ ] 1.3 Implement save/cancel functionality

- [ ] 2.0 Implement avatar upload feature
  - [ ] 2.1 Create file upload component
  - [ ] 2.2 Add image preview functionality
  - [ ] 2.3 Implement image compression and validation

- [ ] 3.0 Integrate with backend API
  - [ ] 3.1 Create profile update endpoint
  - [ ] 3.2 Add error handling for API failures
  - [ ] 3.3 Implement loading states
```

### Step 3: Implement Tasks Systematically

**Goal**: Implement features step-by-step with proper testing and version control.

#### How to Use `process-task-list.md`

1. **Start with the first task**:
   ```text
   Please start on task 1.1 and use @process-task-list.md
   ```

2. **Review each completed sub-task**:
   - AI marks sub-tasks as `[x]` when complete
   - AI runs tests and commits changes
   - You approve before moving to next task

3. **Monitor progress** as tasks are systematically completed

## Tool-Specific Setup Guides

### Cursor Setup

**Easiest option** - works out of the box:

1. Copy the three `.md` files to your project root
2. Reference them with `@` in Cursor's Agent chat:
   ```text
   Use @create-prd.md to help me plan a new feature
   ```

**Pro Tips for Cursor:**
- Use **MAX mode** for PRD creation for more thorough results
- Consider upgrading to Pro plan for better AI model capabilities

### Claude Code Setup

**For systematic development across projects:**

1. **Copy files to your repo**:
   ```bash
   mkdir ai-dev-tasks
   cp create-prd.md generate-tasks.md process-task-list.md ai-dev-tasks/
   ```

2. **Add to CLAUDE.md**:
   ```markdown
   # AI Dev Tasks
   Use these files for structured feature development:
   /ai-dev-tasks/create-prd.md
   /ai-dev-tasks/generate-tasks.md
   /ai-dev-tasks/process-task-list.md
   ```

3. **Create custom commands** (optional):
   ```bash
   mkdir -p .claude/commands/
   # Add command files for quick access
   ```

### Other AI Tools

**Universal approach:**
1. Copy the `.md` files to your project
2. Reference them according to your tool's documentation
3. Follow the same workflow principles

## Real-World Example: Building a Task Management Feature

Let's walk through a complete example of building a task management feature:

### 1. Create the PRD

```text
You: Use @create-prd.md
I want to build a task management feature where users can create, edit, and track their daily tasks.

AI: I'll help you create a PRD. First, I need to ask some clarifying questions:
1. What problem does this solve for users?
2. Who are the primary users?
3. What core actions should users be able to perform?
```

**Answers provided:**
1. Helps users organize their work and stay productive
2. Busy professionals who need to track multiple tasks
3. Create tasks, mark as complete, categorize by project, set due dates

**Generated PRD:** `0001-prd-task-management.md`

### 2. Generate Tasks

```text
You: Take @0001-prd-task-management.md and create tasks using @generate-tasks.md

AI: I have generated the high-level tasks based on the PRD. Ready to generate sub-tasks? Respond with 'Go'
You: Go

AI: Here are the detailed tasks...
```

**Generated Task List:** `tasks-0001-prd-task-management.md`

### 3. Implement Systematically

```text
You: Please start on task 1.1 and use @process-task-list.md

AI: I'll implement task 1.1: Create TaskList component with basic layout
[implements component, runs tests, commits changes]

Task 1.1 complete. Ready for next task? (y/n)
You: y
```

## Advanced Patterns and Best Practices

### Pattern 1: Complex Feature Breakdown

For complex features, use this structure:

```
0001-prd-payment-system.md
├── tasks-0001-prd-payment-system.md (main implementation)
├── 0002-prd-payment-integrations.md (follow-up enhancements)
└── tasks-0002-prd-payment-integrations.md
```

### Pattern 2: Iterative Development

For phased rollouts:

```markdown
## Tasks

- [ ] 1.0 Phase 1: Basic task creation
  - [ ] 1.1 Create task input form
  - [ ] 1.2 Add task storage (localStorage initially)

- [ ] 2.0 Phase 2: Task persistence
  - [ ] 2.1 Implement database storage
  - [ ] 2.2 Add data migration logic

- [ ] 3.0 Phase 3: Advanced features
  - [ ] 3.1 Add task categories
  - [ ] 3.2 Implement due date functionality
```

### Best Practice 1: Start Small

- Begin with one feature at a time
- Use AI Dev Tasks for new features or significant changes
- Don't try to spec your entire application at once

### Best Practice 2: Be Specific in Requirements

- Provide concrete examples in your PRD
- Include both happy path and edge cases
- Specify non-functional requirements (performance, security)

### Best Practice 3: Regular Checkpoints

- Review each completed task before proceeding
- Run full test suites after major changes
- Use meaningful commit messages

## Troubleshooting Common Issues

### Problem: AI Doesn't Follow the Workflow
**Solution**: Ensure all three `.md` files are properly referenced and accessible to your AI tool.

### Problem: Tasks Are Too Large or Vague
**Solution**: Ask the AI to break tasks into smaller sub-tasks:
```
Can you break task 2.1 into more specific sub-tasks?
```

### Problem: Implementation Quality Issues
**Solution**: Use the step-by-step approach and review each task:
- Don't rush through tasks
- Test thoroughly before approving
- Provide feedback for corrections

## Comparison: AI Dev Tasks vs Other Tools

| Tool | Focus | AI Dev Tasks Advantage |
|------|-------|------------------------|
| **OpenSpec** | Specification management | Feature development workflow |
| **Traditional AI** | Code generation | Structured, verifiable process |
| **Manual Development** | Human planning | AI leverage with human oversight |

## Integration with Development Workflows

### Git Workflow Integration

```bash
# After each parent task completion
git add .
git commit -m "feat: implement user authentication" \
           -m "- Add login/logout components" \
           -m "- Integrate with auth service" \
           -m "Related to task 1.0 in PRD-0001"
```

### Testing Integration

- Run tests after each parent task
- Use the "Relevant Files" section to track test coverage
- Ensure all edge cases are covered

## Success Metrics and Measurement

Track these metrics to measure AI Dev Tasks effectiveness:

- **Completion rate**: Percentage of tasks completed successfully
- **Cycle time**: Time from PRD creation to feature completion
- **Error rate**: Number of rollbacks or major corrections needed
- **Code quality**: Test coverage and maintainability scores

## Conclusion

AI Dev Tasks transforms unpredictable AI-assisted development into a structured, reliable process. By breaking complex features into manageable tasks with built-in checkpoints, you maintain control while leveraging AI's capabilities.

The system works with any AI coding assistant and scales from simple features to complex applications. Start with the basic workflow, then adapt the `.md` files to match your team's preferences and project requirements.

## Resources

- **Repository**: [github.com/snarktank/ai-dev-tasks](https://github.com/snarktank/ai-dev-tasks)
- **Video Demo**: [How I AI Podcast Episode](https://www.youtube.com/watch?v=fD4ktSkNCw4)
- **Issues**: [Report problems or request features](https://github.com/snarktank/ai-dev-tasks/issues)

## References

1. **SnarkTank**. (2024). *AI Dev Tasks: Structured Feature Development with AI Assistants*. GitHub Repository. https://github.com/snarktank/ai-dev-tasks

---

*This tutorial covers the complete AI Dev Tasks workflow and is designed to help developers effectively use AI assistants for feature development.*
