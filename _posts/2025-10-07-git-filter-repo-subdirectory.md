---
layout: single
title: Git Move Subdirectory to New Repo
tags:
  - git
  - filter-repo
  - subtree
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Two methods to move subdirectory to new repo with full history - git filter-repo and git subtree split
---

## TL;DR

Two ways to move a subdirectory to a new repository while preserving commit history:
- **git subtree split** - Safe, non-destructive, built-in
- **git filter-repo** - Powerful, requires installation, rewrites history

## Why This Rocks

I was working on a monorepo and needed to split out a subdirectory into its own repository. Found two great methods - one safe and built-in, another more powerful but requires external tools.

## Method 1: Git Subtree Split (Safe & Built-in)

This is the safer, non-destructive method using built-in Git commands.

### Step 1: Create New Branch with Subdirectory

In your original repository:

```bash
git subtree split --prefix=<path/to/your/subdirectory> -b <new-branch-name>
```

For example, to split `my-app` into a branch called `my-app-split`:
```bash
git subtree split --prefix=my-app -b my-app-split
```

### Step 2: Create New Remote Repository

Create a new empty repository on GitHub/GitLab and get its URL.

### Step 3: Push Branch to New Repo

```bash
git push <new-repo-url> <new-branch-name>:main
```

Example:
```bash
git push https://github.com/user/new-app-repo.git my-app-split:main
```

### Step 4: Clean Up

Remove the temporary branch from original repo:
```bash
git branch -D my-app-split
```

## Method 2: Git Filter-Repo (Powerful & Clean)

This method rewrites history but gives cleaner results.

### Step 1: Install git-filter-repo

```bash
pip install git-filter-repo
```

*On macOS you can also use `brew install git-filter-repo`*

### Step 2: Clone Fresh Copy

```bash
git clone --mirror <original-repo-url> temp-repo
cd temp-repo
```

### Step 3: Filter History

```bash
git filter-repo --path <path/to/your/subdirectory>
```

For example, if your subdirectory is `my-app`:
```bash
git filter-repo --path my-app/
```

### Step 4: Create New Remote

Create a new empty repository on GitHub/GitLab and get its URL.

### Step 5: Push Filtered History

```bash
git remote set-url origin <new-repo-url>
git push --mirror
```

That's it! Your new repo now has only the subdirectory and its complete history.

## Cleanup (Optional)

To remove the subdirectory from the original repo:

```bash
# In fresh clone of original repo
git filter-repo --invert-paths --path <path/to/your/subdirectory>
git push origin --force --all
```

## Which Method to Choose?

| Feature | `git subtree split` | `git filter-repo` |
| :--- | :--- | :--- |
| **Safety** | ✅ **Non-destructive** - Creates new branch, leaves original untouched | ⚠️ **Rewrites history** - Must use on clone |
| **Installation** | ✅ **Built-in** - No extra tools needed | ❌ **Requires install** - `pip install git-filter-repo` |
| **Process** | Creates branch in existing repo, push to new remote | Creates new repo folder locally, then push |
| **Best for** | **Quick, safe splits** without touching original | **Clean history** and complex migrations |

**Bottom line:** Use `git subtree split` for safe, simple splits. Use `git filter-repo` when you need the most powerful tool and plan to clean up the original repo too.

## References

* [git-filter-repo docs](https://github.com/newren/git-filter-repo)
* [Official tutorial](https://htmlpreview.github.io/?https://github.com/newren/git-filter-repo/blob/docs/html/git-filter-repo.html)
