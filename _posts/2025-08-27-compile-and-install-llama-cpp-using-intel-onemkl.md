---
layout: single
title: Compile and Install llama.cpp using Intel OneMKL
tags:
  - llama.cpp
  - Intel
  - OneMKL
  - AI
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
excerpt: How to compile and install llama.cpp using Intel OneMKL for better performance on Intel CPUs.
---

Building `llama.cpp` with Intel OneMKL can provide significant performance improvements on Intel CPUs by utilizing the AVX_VNNI instruction set, even on processors that do not support AVX512. This guide will walk you through the process.

Please note that this build configuration **does not support Intel GPU**. For Intel GPU support, please refer to [llama.cpp for SYCL](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/SYCL.md).

### My CPU

I am using CPU with `avx_vnni`. That's why I want to use this.

### Install Intel OneMKL Library.

NOTE: This will install a total of 17GB of toolkits.

These commands are based on the article in [Intel OneMKL offline installation via command line](https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=oneapi-toolkit&oneapi-toolkit-os=linux&oneapi-lin=offline)


I added `-c` in the `wget` command so we can continue if we get disconnected from the download.

```bash
wget -c https://registrationcenter-download.intel.com/akdlm/IRC_NAS/3b7a16b3-a7b0-460f-be16-de0d64fa6b1e/intel-oneapi-base-toolkit-2025.2.1.44_offline.sh

sudo sh ./intel-oneapi-base-toolkit-2025.2.1.44_offline.sh -a --silent --cli --eula accept
```

For detailed and up-to-date requirements, please refer to the official [Intel oneAPI Base Toolkit documentation](https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html).

### Install Dependencies

I'm using Ubuntu:
```
sudo apt update
sudo apt -y install cmake pkg-config build-essential libjemalloc-dev
sudo apt -y install libcurlpp-dev
```

I'm using Jemalloc for better memory handling.
The CURL library is important.

### Compile App

By default, `GGML_BLAS_VENDOR` is set to `Generic`, so if you have already sourced the Intel environment script and set `-DGGML_BLAS=ON` in CMake, the MKL version of BLAS will be automatically selected. Otherwise, you will need to install oneAPI and follow these steps:

```bash
source /opt/intel/oneapi/setvars.sh # This step can be skipped if you are in a oneapi-basekit docker image.
cmake -B build -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=Intel10_64lp -DCMAKE_C_COMPILER=icx -DCMAKE_CXX_COMPILER=icpx -DGGML_NATIVE=ON -DCMAKE_EXE_LINKER_FLAGS="-ljemalloc"
cmake --build build --config Release -j$(nproc)
```

Check for Jemalloc:

```bash
$ ldd ./build/bin/llama-cli | grep jemalloc
        libjemalloc.so.2 => /lib/x86_64-linux-gnu/libjemalloc.so.2 (0x000074ba0fc00000)
```

### Install

It's a little bit weird that Llama.cpp doesn't provide installation guide. They have so many libraries, including the python binding. But, for now, let's install the binaries first.

```bash
mkdir -p ~/.local/share/llama.cpp
cp -a build/bin/ ~/.local/share/llama.cpp/
echo 'export PATH=$PATH:$HOME/.local/share/llama.cpp/bin' >> ~/.bashrc 
source ~/.bashrc
```

Check:

```bash
$ llama-cli --version
version: 6294 (bcbddcd5)
built with Intel(R) oneAPI DPC++/C++ Compiler 2025.2.1 (2025.2.0.20250806) for x86_64-unknown-linux-gnu
```