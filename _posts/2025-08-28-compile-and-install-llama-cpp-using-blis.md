---
layout: single
title: Compile and Install llama.cpp using BLIS
tags:
  - llama.cpp
  - BLIS
  - AI
  - fedora
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
excerpt: How to compile and install llama.cpp using BLIS for better performance on old Ryzen laptop.
---

### Install BLIS

Get BLIS:

```bash
git clone https://github.com/flame/blis && cd blis
```

This command clones the BLIS (BLAS-like Library Instantiation Software) repository from GitHub and navigates into the cloned directory. BLIS is a high-performance BLAS library optimized for various CPU architectures.

Compile and install

```bash
CFLAGS="-O3 -march=native -mtune=native -funroll-loops -fomit-frame-pointer" LDFLAGS="-ljemalloc" ./configure --prefix=/usr --libdir=/usr/lib64 --enable-cblas  -t openmp,pthreads zen2
```

This configure command sets up BLIS for compilation with optimizations for Zen 2 architecture (used in Ryzen processors). It enables CBLAS interface, OpenMP and pthreads threading, and installs to system directories. The CFLAGS optimize for performance with native architecture tuning and jemalloc for memory allocation.

NOTE:
I install them in `/usr` because `llama-cli` doesn't linked to `/usr/local/lib/libblis.so`


## Install `llama.cpp`

Get Llama.cpp:

```bash
cd ..
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
```

Let's compile it:

```bash
cmake -B build -DCMAKE_BUILD_TYPE=Release -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=FLAME  -DCMAKE_C_FLAGS="-O3 -march=znver2 -mtune=znver2 -fPIC" -DCMAKE_CXX_FLAGS="-O3 -march=znver2 -mtune=znver2 -fPIC" -DCMAKE_EXE_LINKER_FLAGS="-lblis -ljemalloc"

cmake --build build --config Release
```

