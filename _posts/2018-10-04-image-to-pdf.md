---
layout: single
title: Image to PDF
tags:
  - pdf
  - image
categories:
  - tools
  - tutorial
  - linux
excerpt: "How to convert multiple images into a single PDF file using ImageMagick convert command on Linux systems."
---

Adding images to pdf:

```sh

convert 1.jpg 2.jpg output.pdf

```

Compress:

```sh
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dQUIET -dBATCH -sOutputFile=input.pdf output.pdf
```

Another way to compress:

```sh
ps2pdf input.pdf output.pdf
```