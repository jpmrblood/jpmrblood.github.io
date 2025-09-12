---
layout: single
title: Compile and Install llama.cpp using Intel OneMKL
tags:
  - llama.cpp
  - Intel
  - OneMKL
  - ai
categories:
  - ai
  - linux
  - performance
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
excerpt: How to compile and install llama.cpp using Intel OneMKL for better performance on Intel CPUs.
---

Building `llama.cpp` with Intel OneMKL can provide significant performance improvements on Intel CPUs by utilizing the AVX_VNNI instruction set, even on processors that do not support AVX512. This guide will walk you through the process.

Please note that this build configuration **does not support Intel GPU**. For Intel GPU support, please refer to [llama.cpp for SYCL](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/SYCL.md).

### My CPU

I am using a DELL Latitude 13" 7340 I7 1365 with 16GB of RAM. It has Intel Iris and vPro extension. My CPU only comes with `avx avx2 avx_vnni` extensions. This ain't CUDA nor ROCm. Fortunately, Intel has something to offer.

### Install Intel OneMKL Library.

NOTE: This will install a total of 17GB of toolkits.

These commands are based on the article in [Intel OneMKL offline installation via command line](https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=oneapi-toolkit&oneapi-toolkit-os=linux&oneapi-lin=offline)


I added `-c` in the `wget` command so we can continue if we get disconnected from the download.

```bash
wget -c https://registrationcenter-download.intel.com/akdlm/IRC_NAS/3b7a16b3-a7b0-460f-be16-de0d64fa6b1e/intel-oneapi-base-toolkit-2025.2.1.44_offline.sh

sudo sh ./intel-oneapi-base-toolkit-2025.2.1.44_offline.sh -a --silent --cli --eula accept
```

For detailed and up-to-date requirements, please refer to the official [Intel oneAPI Base Toolkit documentation](https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html).

### Install Additional Dependencies

I'm using Ubuntu 25.04, but this is actually commands for other versions of Ubuntu.

```bash
sudo apt update
sudo apt -y install cmake pkg-config build-essential
sudo apt -y install libcurlpp-dev
sudo apt -y install libjemalloc-dev
```

• `cmake`: Build system generator for configuring and compiling llama.cpp projects.
• `pkg-config`: Tool for managing library compile/link flags, essential for dependency
resolution.
• `build-essential`: Meta-package providing core build tools (gcc, g++, make) for C/C++ compilation.
• `libcurlpp-dev`: C++ wrapper for libcurl, enabling HTTP requests (e.g., downloading models from Hugging Face).
• `libjemalloc-dev`: Development headers for jemalloc, a high-performance memory allocator for better memory management. 

NOTE: I did a comparison between `jemalloc`, `tcmalloc`, `hoard`, `ptmalloc` few years back and found out that `jemalloc` is the most stable and powerful of all for managing memory. Memory fragmentation is the enemy of app with exhaustive memory usage like `llama.cpp`.


### Compile App

By default, `GGML_BLAS_VENDOR` is set to `Generic`, so if you have already sourced the Intel environment script and set `-DGGML_BLAS=ON` in CMake, the MKL version of BLAS will be automatically selected. Otherwise, you will need to install oneAPI and follow these steps:

```bash
source /opt/intel/oneapi/setvars.sh 
cmake -B build -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=Intel10_64lp -DCMAKE_C_COMPILER=icx -DCMAKE_CXX_COMPILER=icpx -DGGML_NATIVE=ON -DCMAKE_EXE_LINKER_FLAGS="-ljemalloc"
cmake --build build --config Release -j$(nproc)
```

Check for Jemalloc:

```bash
$ ldd ./build/bin/llama-cli | grep jemalloc
        libjemalloc.so.2 => /lib/x86_64-linux-gnu/libjemalloc.so.2 (0x000074ba0fc00000)
```

### Installing the Binaries

While `llama.cpp` does not provide a standard installation script (`make install`), you need to manually place the compiled binaries. I opt for put the `bin` directory in `build` into a local directory and add it to your system's `PATH`. The reason why I don't put it in the general `bin` directory is because it has a lot of binaries and libraries.

Here’s a common way to do it:

1.  **Create a directory** to store the `llama.cpp` binaries. We'll use `~/.local/share/llama.cpp`.

    ```bash
    mkdir -p ~/.local/share/llama.cpp
    ```

2.  **Copy the compiled binaries** from the `build/bin` directory to the newly created directory.

    ```bash
    cp -a build/bin/ ~/.local/share/llama.cpp/
    ```

3.  **Add the new directory to your `PATH`**. This command appends a line to your `.bashrc` file, so the `PATH` is updated automatically in future terminal sessions.

    ```bash
    echo 'export PATH=$PATH:$HOME/.local/share/llama.cpp/bin' >> ~/.bashrc 
    ```

4.  **Apply the changes** to your current terminal session.

    ```bash
    source ~/.bashrc
    ```

Now, you can verify that the installation was successful by checking the version of `llama-cli`:

```bash
$ llama-cli --version
version: 6294 (bcbddcd5)
built with Intel(R) oneAPI DPC++/C++ Compiler 2025.2.1 (2025.2.0.20250806) for x86_64-unknown-linux-gnu
```

The output confirms that the `llama-cli` command is now accessible and that it was compiled with the Intel compiler.

### Running a Model

With `llama.cpp` installed, you can now run a model. The `llama-cli` tool can automatically download a model from Hugging Face and run it.

If you are using a new terminal, you need to setup Intel OneMKL libraries once:

```bash
source /opt/intel/oneapi/setvars.sh
```

Now you are ready to run the mode. Here is an example command to run Qwen/Qwen3-8B-GGUF:

```bash
llama-cli -hf Qwen/Qwen3-8B-GGUF:Q4_K_M --jinja --color -c 4096 -n 1024 -t 8 --temp 0.6 --presence-penalty 1.5 --no-context-shift
```

Here’s a breakdown of the parameters used in this command:

-   `-hf Qwen/Qwen3-8B-GGUF:Q4_K_M`: Downloads the model from the Hugging Face repository `Qwen/Qwen3-8B-GGUF` using the `Q4_K_M` quantization.
-   `--jinja`: Uses the Jinja2 chat template for formatting the prompt, which is required for some models.
-   `--color`: Enables colorized output for better readability.
-   `-c 4096`: Sets the context size to 4096 tokens.
-   `-n 1024`: Sets the maximum number of tokens to generate to 1024.
-   `-t 8`: Uses 8 threads for processing.
-   `--temp 0.6`: Sets the temperature for sampling. A lower value makes the output more deterministic.
-   `--presence-penalty 1.5`: Applies a penalty to repeated tokens to encourage the model to generate more diverse text.
-   `--no-context-shift`: Prevents the context from being shifted when the prompt is too long.
