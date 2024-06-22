---
title: 7zip extract zip from CMD
tags:
  - 7z
  - windows
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Extract zip file 
---

Run the command:
```
"C:\Program Files\7-Zip\7z.exe" x D:\source_dir\source.zip -oOUTPUT_DIR -mmt8 -slp
```

- use `-mmt8` to have 8 threads.
- use `-slp` to use Windows' Large Page memory