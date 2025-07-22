---
layout: single
title: Make New GIT Branch from Commit ID
tags:
  - git
  - branch
  - cherrypick
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Make a New Git Branch from Commit ID
---
If you want to create a new branch from an older commit (not the latest one), here's how:

# ✅ Steps:

Find the commit hash:

```bash
git log --oneline
```
Example output:

```sql
e3a1b55 Fix security issue
6f24a1d Add README
34d2a5c Initial commit
```
Create a branch from that commit:

```bash
git checkout -b my-old-branch e3a1b55
```
Note:
* Replace my-old-branch with your desired branch name.
* Replace e3a1b55 with the actual commit hash.

Push the new branch (optional):

```bash
git push origin my-old-branch
```

# 🔄 If you're already on another branch and don’t want to switch:

You can also do this without switching branches:

```bash
git branch my-old-branch e3a1b55
```

Then push:

```bash
git push origin my-old-branch
```