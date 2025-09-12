---
layout: single
title: Change DOS to UNIX Line Ending
tags:
  - bash
categories:
  - linux
  - programming
  - tutorial
excerpt: "Convert DOS/Windows line endings (CRLF) to Unix/Linux format (LF) using command line tools and Vim for batch processing."
---
How to change line ending format

```sh
for file in *.cpp
do 
    vi +':w ++ff=unix' +':q' "$file"
done
```

Source:
https://stackoverflow.com/a/95650