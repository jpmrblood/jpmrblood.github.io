---
layout: single
title: "Prompting Techniques Cheatsheet"
tags:
  - prompting
  - llm
  - ai
  - techniques
  - cheatsheet
categories:
  - tech
  - programming
  - ai-tools
excerpt: "A practical cheat sheet for various prompting techniques like Zero-Shot, Few-Shot, Chain of Thought, and more, with copy-paste templates."
---

**TL;DR**:

- **Zero-Shot & Few-Shot**: Use for simple tasks or to provide the model with examples to mimic.
- **Chain of Thought (CoT)**: Best for complex tasks that require step-by-step reasoning.
- **Tree of Thoughts (ToT)**: Ideal for comparing multiple design choices or paths.
- **Retrieval-Augmented Generation (RAG)**: Use when accuracy and up-to-date information from external documents are critical.
- **Directional Stimulus**: Apply when the tone, style, or perspective of the output is important.
- **Self-Refinement**: Use for iterative tasks where the model should review and improve its own output.

# 📝 Prompting Techniques Cheat Sheet

## 1. 🔵 Zero-Shot Prompting

**When:** Simple tasks, trust model’s training.
**Pattern:**

```
Do [task] about [topic]. 
Requirements: [list constraints].
```

**Example:**

```
Create a landing page in Vite for an Agricultural Government Agency. 
Use Tailwind utility classes only.
```

---

## 2. 🟢 Few-Shot Prompting

**When:** Want model to mimic examples.
**Pattern:**

```
Example 1: [input → output]  
Example 2: [input → output]  
Now, do the same for [new input].
```

**Example:**

```
Example 1: Create a landing page in Vite for a Health Department.  
Example 2: Create a landing page in Vite for an Education Ministry.  
Now, create one for an Agricultural Government Agency.
```

---

## 3. 🟡 Chain of Thought (CoT)

**When:** Complex, step-by-step reasoning needed.
**Pattern:**

```
Step 1: [break down]  
Step 2: [analyze]  
Step 3: [generate output].
```

**Example:**

```
Step 1: Summarize Unbounce’s landing page principles.  
Step 2: Outline sections for an Agricultural Agency landing page.  
Step 3: Generate the Vite + Tailwind code.
```

---

## 4. 🟠 Tree of Thoughts (ToT)

**When:** Multiple design choices, comparisons.
**Pattern:**

```
Option A: [path] → Pros/Cons  
Option B: [path] → Pros/Cons  
Option C: [path] → Pros/Cons  
Pick best option → implement.
```

**Example:**

```
Give 3 layout styles for the landing page (formal, citizen-focused, modern storytelling). 
Compare them, then build the best in Vite + Tailwind.
```

---

## 5. 🔴 Retrieval-Augmented Generation (RAG)

**When:** Accuracy, up-to-date info.
**Pattern:**

```
Use information from [docs/URL].  
Do [task] with correctness based on these sources.
```

**Example:**

```
Use Tailwind CSS docs and Unbounce principles. 
Create a Vite landing page styled with Tailwind that follows those guidelines.
```

---

## 6. 🟣 Directional Stimulus Prompting

**When:** Tone/style/audience matters.
**Pattern:**

```
Do [task] in the style of [tone/perspective]. 
Emphasize [key aspects].
```

**Example:**

```
Create a Vite landing page for an Agricultural Government Agency. 
Tone: authoritative but approachable. 
Highlight sustainability, food security, and farmer support.
```

---

## 7. 🟤 Self-Refinement Prompting

**When:** Want iteration/improvement.
**Pattern:**

```
Do [task].  
Now review: Does it meet [criteria]?  
If not, improve and refine until it does.
```

**Example:**

```
Build a landing page in Vite + Tailwind.  
Review: Does it have clear CTA, scannable sections, trust signals?  
If not, refine and fix until ready.
```

---

⚡ **Pro Tip for Coding:** Always include:

* 🔧 Setup requirements (`tailwind.config.js`, imports)
* 🎨 Styling method (utility classes only, no inline)
* 📦 Output format (full project, single file, snippet)

---
