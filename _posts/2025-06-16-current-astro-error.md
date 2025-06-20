---
layout: single
title: Current Astro Bug
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
excerpt: Today's Astro bug
---
This might go away.
Bug:

```
bun create astro@latest my-project
cd my-project && bun dev
```

The expected result should be running okay. However, I got this:
```bash
18.11.05 [ERROR] [UnhandledRejection] Astro detected an unhandled rejection. Here's the stack trace:
Error [ERR_MODULE_NOT_FOUND]: Cannot find package '@shikijs/langs' imported from /home/jp/PERURI-ID/xpass-bgn/frontend/auth-static/node_modules/shiki/dist/langs.mjs
    at Object.getPackageJSONURL (node:internal/modules/package_json_reader:268:9)
    at packageResolve (node:internal/modules/esm/resolve:768:81)
    at moduleResolve (node:internal/modules/esm/resolve:854:18)
    at defaultResolve (node:internal/modules/esm/resolve:984:11)
    at ModuleLoader.defaultResolve (node:internal/modules/esm/loader:685:12)
    at #cachedDefaultResolve (node:internal/modules/esm/loader:634:25)
    at ModuleLoader.resolve (node:internal/modules/esm/loader:617:38)
    at ModuleLoader.getModuleJobForImport (node:internal/modules/esm/loader:273:38)
    at onImport.tracePromise.__proto__ (node:internal/modules/esm/loader:577:36)
    at TracingChannel.tracePromise (node:diagnostics_channel:344:14)
  Hint:
    Make sure your promises all have an `await` or a `.catch()` handler.
  Error reference:
    https://docs.astro.build/en/reference/errors/unhandled-rejection/
  Stack trace:
    at Object.getPackageJSONURL (node:internal/modules/package_json_reader:268:9)
    at moduleResolve (node:internal/modules/esm/resolve:854:18)
    at ModuleLoader.defaultResolve (node:internal/modules/esm/loader:685:12)
    at ModuleLoader.resolve (node:internal/modules/esm/loader:617:38)
    at onImport.tracePromise.__proto__ (node:internal/modules/esm/loader:577:36)
    [...] See full stack trace in the browser, or rerun with --verbose.
18.19.38 [200] / 42ms
```

According to ChatGPT, the newer versions of Shiki (like 1.0.0+) made @shikijs/langs an external package to slim down the core library.

The fix:

```bash
bun i -d @shikijs/langs
```

This probably temporary error.