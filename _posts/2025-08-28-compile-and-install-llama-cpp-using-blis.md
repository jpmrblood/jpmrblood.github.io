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

Today I want to test my luck to run `llama.cpp` in  ThinkPad L14 Gen 1 in Fedora Linux 42. I have 32GB of RAM, mind you, so I can try to load bigger model.

### Install BLIS

Get BLIS:

```bash
git clone https://github.com/flame/blis && cd blis
```

This command clones the BLIS (BLAS-like Library Instantiation Software) repository from GitHub and navigates into the cloned directory. BLIS is a high-performance BLAS library optimized for various CPU architectures.

Compile and install

```bash
CFLAGS="-O3 -march=native -mtune=native -funroll-loops -fomit-frame-pointer" LDFLAGS="-ljemalloc" ./configure --prefix=/usr --libdir=/usr/lib64 --enable-cblas  -t openmp,pthreads zen2
sudo make install
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

### Installing the Binaries

While `llama.cpp` does not provide a standard installation script (`make install`), you need to manually place the compiled binaries. I opt for put the `bin` directory in `build` into a local directory and add it to your system's `PATH`. The reason why I don't put it in the general `bin` directory is because it has a lot of binaries and libraries.

Here's a common way to do it:

1. **Create a directory** to store the `llama.cpp` binaries. We'll use `~/.local/share/llama.cpp`.

   ```bash
   mkdir -p ~/.local/share/llama.cpp
   ```

2. **Copy the compiled binaries** from the `build/bin` directory to the newly created directory.

   ```bash
   cp -a build/bin/ ~/.local/share/llama.cpp/
   ```

3. **Add the new directory to your `PATH`**. This command appends a line to your `.bashrc` file, so the `PATH` is updated automatically in future terminal sessions.

   ```bash
   echo 'export PATH=$PATH:$HOME/.local/share/llama.cpp/bin' >> ~/.bashrc
   ```

4. **Apply the changes** to your current terminal session.

   ```bash
   source ~/.bashrc
   ```

Now, you can verify that the installation was successful by checking the version of `llama-cli`:

```bash
$ llama-cli --version
version: 6294 (bcbddcd5)
built with gcc (GCC) 14.2.1 20240805 (Red Hat 14.2.1-1) for x86_64-redhat-linux
```

The output confirms that the `llama-cli` command is now accessible and that it was compiled with GCC.

### Running a Model

With `llama.cpp` installed, you can now run a model. The `llama-cli` tool can automatically download a model from Hugging Face and run it.

Here is an example command to run a model:

```bash
llama-cli -hf unsloth/Qwen3-4B-Instruct-2507-GGUF:Q4_K_M --color -c 2048 -n 512 -t 3 --temp 0.7
```

Here's a breakdown of the parameters used in this command:

- `-hf unsloth/Qwen3-4B-Instruct-2507-GGUF:Q4_K_M`: Downloads the Qwen3-4B-Instruct model (quantized to Q4_K_M) from the Hugging Face repository `unsloth/Qwen3-4B-Instruct-2507-GGUF`
- `--color`: Enables colorized output for better readability
- `-c 2048`: Sets the context size to 2048 tokens (conservative for laptop memory)
- `-n 512`: Sets the maximum number of tokens to generate to 512 (reasonable response length)
- `-t 3`: Uses 3 threads for processing (conservative for laptop thermal management)
- `--temp 0.7`: Sets the temperature for sampling (balanced creativity vs determinism)

