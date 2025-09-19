---
layout: single
title: "My Experience Using GitHub Spec Kit with OpenCode and Qwen Code CLI"
date: 2025-09-17
tags:
  - spec-kit
  - github
  - opencode
  - qwen
  - ai
  - development
  - integration
  - personal-experience
  - animejs
  - animation
categories:
  - ai
  - tools
  - development
excerpt: "A personal account of integrating GitHub Spec Kit with OpenCode and Qwen Code CLI for enhanced spec-driven development, featuring a practical AnimeJS presentation webpage project and lessons learned."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

**TL;DR**:

**TBD**

## Fixing Problem

From my experience, I can say that Github Spec Kit is one sprint method. For complex project, it sometimes lost and suddenly it won't follow the constitution anymore. Worst, it would get lost in limbo printing the same thing over and over again. Maybe the model for free tier is not that powerful.

[Eberie suggest to create a PRD.](https://steviee.medium.com/from-prd-to-production-my-spec-kit-workflow-for-structured-development-d9bf6631d647) What a good idea. However, I noticed that he manually runs the tasks and read the template. He even forced the Agent to create a preparation step.

This breaks the convention and it doesn't sit well with me. What he did probably will work in Claude because Claude is smart enough to understand. I can't do that. A workflow should not have any exception. As CMMI Dev suggested, you don't go outside your workflow; you fix your workflow to be able to do the biddings. Or in the terms of CMMI Dev: Do continous improvements.

## How it goes

For a freebies like me, I would like to combine these:
- Creating specifications, planning, etc. using Deepseek and ChatGPT websites.
- Doing the Github Spec Kit workflow with Opencode CLI.
- Implement the code using Qwen Code CLI.

So, I would like to document what I did.

## MCP Involved

Only one MCP involved: Context7.
The web search tool is already baked inside of Opencode and Qwen CLI so I don't have the need to enable a search MCP anymore.

I just need to create a following in the constitution to use them.

## Setup Github Spec Kit

[Setup Github Spec Kit with Opencode](2025-09-16-using-github-spec-kit-with-opencode.md)


## One Little Thing I Do

I prefer the convention.

> get commit message according to https://www.conventionalcommits.org/en/v1.0.0/#specification including the initial commit message

It will modify initial commit message and the messages afterward.


## Create PRD

How I get the PRD is by asking Deepseek.


> Create a PRD in markdown format. For this requirements:
> I want to make a SPA webpage that can be run locally. The purpose of this webpage is to render presentations with animation and cool page transitions. I can use my arrow buttons in keyboard for navigation.

Make some iteration to include things that you forgot to mentioned.

The final PRD document:
<script src="https://gist.github.com/jpmrblood/dabdcf1e6d641ba92c7e22b4e8f9fc00.js"></script>

I save it in the project's root folder

## Create The Constitution

I don't  touch `.specify/memory/constitution_update_checklist.md` file. It seems [the tutorial](https://www.youtube.com/watch?v=a9eR1xsfvHg) also doesn't touch it. What I did was to edit the `.specify/memory/constitution.md`. The first line written manually while the rest is copy and paste.

```markdown
update @.specify/memory/constitution.md based on its template. Fill the basic core principles needed for this project to achieve the obejctives in @PRD.md.

Additionally, have the development follow TDD principle with the following:
1. ALWAYS use Context7 for latest documentation for testing and the implementation, if it's not enough, do web search. If you have not found anything, STOP and ask my opinion how to move forward.
2. Create a working unit test for this task.
3. Implement the requirement sets by the task.
4. Verify the implementation with the unit test.
5. IF not working, get back to #1. MAXIMUM retry is 5 before you stop and ASK me how to proceed.
6. Commit the work after each successful objective (plan, task, etc.) using the convention. 
Apply SOLID principle when it's applicable.

```

## Create The Base

As I mentioned in [my LinkedIn post](https://www.linkedin.com/pulse/current-workflow-vibe-coding-jan-peter-alexander-knwoc/), the first pass of the development journey is to create a baseline (boilerplate) for the project to build upon. Eberie did this by creating a special flow `foundational feature 000` outside the Github Spec Kit flow.

I try to create it properly using the Github Spec Kit. I understand his thought, though. Using normal feature burns the token! Better create yourself or use something else.

That's why, I think it is best practice to create this base project and then save it somewhere as a boilerplate.

### Specification 

The first line is created by hand because Opencode seems only recognize that if we type it by hand. The rest of the line is pasted.

> **/specify**  Create a 

### Planning and Research

As usual, I asked Deepseek and ChatGPT.

```markdown
/plan According to spec and @PRD.

Additionally, here's the stack:
- **App folder: app/**

- **Frontend Framework**: **React (with Vite, scaffolded using Bun)**  
  - Scaffolding via `bun create vite@latest app --template react --yes`
  - Bun offers ultra-fast dependency installs and script execution  
  - Excellent component reusability for slides  
  - Strong ecosystem  

- **Animation Library**: **GSAP (GreenSock Animation Platform)**  
  - Industry-standard for high-performance animations  
  - Smooth 60fps transitions  
  - Advanced sequencing  

- **Routing**: **React Router**  
  - Manages slide navigation  
  - Supports deep linking to specific slides  

- **Styling**: **CSS-in-JS (Styled-Components)** or **Tailwind CSS**  
  - Enables dynamic, slide-specific styling  
  - Easy theming support  

- **Media Handling**: **Custom hooks for images/videos**  
  - Supports JPG, PNG, SVG, MP4, WebM  
  - Embedded via relative paths  

- **Touch Gestures**: **Hammer.js** or **custom touch listeners**  
  - Enables swipe left/right navigation on touchscreen devices  

- **Fullscreen**: **Browser Fullscreen API**  
  - Provides native fullscreen support without overlays  

- **Offline Function**: **Local asset loading (no service worker)**  
  - Loads all files locally  
  - Ensures offline operation  

- **Build Tool**: **Vite (powered by Bun)**  
  - Fast dev server and optimized builds  
  - Bun executes scripts and bundling with blazing speed  

- **Unit Testing**: **Vitest** (preferred for Vite + Bun)  
  - Runs natively with Bun for maximum speed  
  - Supports fast, isolated component testing  
  - Great DX (watch mode, snapshot testing, coverage)  
  - **Folder Structure**:  
    - `src/__tests__/` → colocate small component tests with source  
    - Example: `src/components/Slide/__tests__/Slide.test.tsx`  

- **Test-Driven Development (TDD)**: **Playwright**  
  - End-to-end browser automation  
  - Perfect for testing navigation, animations, and fullscreen behavior  
  - Can simulate touchscreen gestures and offline mode  
  - Executed via `bun playwright test`  
  - **Folder Structure**:  
    - `tests/e2e/` → dedicated for Playwright specs  
    - Example: `tests/e2e/presentation-navigation.spec.ts`


What questions do you have?
```

The word `what questions do you have?` is a must. This to tell Opencode to clarify when in doubt.

### Task

Do this:


> **/tasks** Follow **@.specify/memory/constitution.md** and make sure each task doesn't exceed your SSE API time limit.  


After this, I exited Opencode and started Qwen Code.

## Adjustments

My prompt:

> Implement all tasks in **@specs/001-setup-base-webapp/tasks.md** following these principles in **@.specify/memory/constitution.md**

And watch Qwen Code doesn't respect the constitution. Stop it by CTRL+C few times. And do the following prompts:

> You didn't consult Context7! this apparent in the way you setup Tailwind 4. It should be as `@import "tailwindcss";`. Also, you didn't follow the constitution. You didn't commit on every successful tasks. The checklist was not ticked in **@.opencode/command/tasks.md**. Restart the development from T001 and FOLLOW the constitution!

This will force Qwen Code to respect the constitution. I applaud for  Tailwind devs inconsistencies. Their inconsistencies create a clue for us to check if Qwen Code really doing it as it told.