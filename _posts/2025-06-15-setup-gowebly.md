---
layout: single
title: Setup Gowebly  
tags:
  - gowebly
  - golang
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Setup Gowebly
---
Gowebly is a stack of Golang Web Framework with HTMX, Tailwind CSS, AlpineJS.

# Prerequisites

## Air
For Hot Reloading

```bash
go install github.com/air-verse/air@latest
```

## Templ
For templating engine

```bash
go install github.com/a-h/templ/cmd/templ@latest
```

# Install

## Run gowebly from Github

```bash
go run github.com/gowebly/gowebly/v3@latest create
```

## Install gowebly binary


```bash
go install github.com/gowebly/gowebly/v3@latest
```

 # Create New Project

```bash
mkdir my-project && cd my-project
gowebly create
```