---
title: "[WIP] How to Install OpenVINO GenAI on Ubuntu 25.04 with oneMKL and jemalloc for Agentic Coding"
excerpt: "Complete guide to build OpenVINO GenAI from source on Ubuntu 25.04 using Intel oneMKL (OneAPI) and jemalloc for high-performance agentic AI applications in a Python virtual environment."
tags: 
    - openvino
    - genai
    - ubuntu
    - python
    - agenticai
    - onemkl
    - jemalloc
    - oneapi
    - tutorial
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/08/robotic-ai.png
  overlay_image: /assets/2025/08/robotic-ai.png
---

# How to Install OpenVINO GenAI on Ubuntu 25.04 with oneMKL and System jemalloc for Agentic Coding

This guide provides a step-by-step process to install **OpenVINO GenAI** from source on **Ubuntu 25.04 (Plucky Puffin)**—an **unsupported distribution**—using a **Python virtual environment**. It includes performance optimizations with **Intel oneMKL (via OneAPI)** and **jemalloc** from Ubuntu's package repository, linked **directly at build time** using `-ljemalloc`. This setup is ideal for **agentic coding** applications requiring high inference throughput and memory efficiency.

> ⚠️ **Note**: Ubuntu 25.04 is not officially supported by OpenVINO. This guide uses the GitHub source build method for full control over compiler and library integration.

---

## Prerequisites

Ensure your system meets:

- **OS**: Ubuntu 25.04 (or beta)
- **Python**: 3.9–3.12 (avoid 3.13 unless confirmed compatible)
- **Disk**: ≥15 GB free
- **RAM**: 16 GB recommended
- **Internet**: Required

Install build tools:

```bash
sudo apt update
sudo apt install -y build-essential cmake git python3-dev python3-venv wget libelf-dev
```

> ✅ **Tip**: Install a supported Python version:
>
> ```bash
> sudo apt install python3.11 python3.11-venv python3.11-dev
> ```

---

## Step 1: Set Up Python Virtual Environment

```bash
mkdir openvino-genai-env && cd openvino-genai-env
python3.11 -m venv venv
source venv/bin/activate
pip install --upgrade pip
```

---

## Step 2: Install Intel oneAPI Base Toolkit (for oneMKL)

Install Intel's optimized math libraries.

### Add Intel Repository

```bash
wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB | gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | sudo tee /etc/apt/sources.list.d/oneapi.list

sudo apt update
sudo apt install -y intel-basekit
```

### Source Environment

```bash
source /opt/intel/oneapi/setvars.sh
```

> 🔗 [Intel oneAPI Base Toolkit](https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit.html)

---

## Step 3: Install System jemalloc (from Ubuntu)

Use the **Ubuntu-provided jemalloc** instead of building from source.

```bash
sudo apt install -y libjemalloc-dev
```

This installs:
- Header: `/usr/include/jemalloc/jemalloc.h`
- Library: `/usr/lib/x86_64-linux-gnu/libjemalloc.so`

The libjemalloc2 is already installed because it is used by many applications such as Firefox.

---

## Step 4: Modify OpenVINO to Link jemalloc Directly

To link `jemalloc` directly into the OpenVINO runtime (not via `LD_PRELOAD`), we must patch the CMake build configuration.

### Clone OpenVINO

```bash
git clone -b 2025.2.0 https://github.com/openvinotoolkit/openvino.git
```

### Patch CMake to Link `-ljemalloc`

Edit the main `CMakeLists.txt` to add jemalloc as a link library:

```bash
# Backup original
cp CMakeLists.txt CMakeLists.txt.bak

# Append jemalloc linking logic
cat >> CMakeLists.txt << 'EOF'

# --- Custom: Link jemalloc directly ---
find_library(JEMALLOC_LIB jemalloc PATHS /usr/lib /usr/lib/x86_64-linux-gnu NO_DEFAULT_PATH)
if(JEMALLOC_LIB)
    message(STATUS "jemalloc found: ${JEMALLOC_LIB}")
    # Link jemalloc to core runtime
    set_property(TARGET ngraph_frontend_manager APPEND PROPERTY INTERFACE_LINK_LIBRARIES ${JEMALLOC_LIB})
    set_property(TARGET ov_runtime APPEND PROPERTY INTERFACE_LINK_LIBRARIES ${JEMALLOC_LIB})
    set_property(TARGET openvino_c APPEND PROPERTY INTERFACE_LINK_LIBRARIES ${JEMALLOC_LIB})
else()
    message(WARNING "jemalloc not found. Install libjemalloc-dev.")
endif()
# ---
EOF
```

> ✅ This links `jemalloc` to key OpenVINO targets: `ov_runtime`, `ngraph_frontend_manager`, and `openvino_c`.

---

## Step 5: Install Build Dependencies

```bash
chmod +x install_build_dependencies.sh
sed -i 's/ubuntu24.04/ubuntu25.04/g' install_build_dependencies.sh || echo "Patch not needed or failed."
./install_build_dependencies.sh
```

---

## Step 6: Build OpenVINO with oneMKL and jemalloc

```bash
mkdir -p build && cd build

source /opt/intel/oneapi/setvars.sh > /dev/null 2>&1

cmake .. \
    -DCMAKE_BUILD_TYPE=Release \
    -DENABLE_PYTHON=ON \
    -DENABLE_WHEEL=ON \
    -DPYTHON_EXECUTABLE=$(which python) \
    -DENABLE_MKL_DNN=ON \
    -DENABLE_INTEL_CPU_MATH=ON \
    -DENABLE_OV_ONNX_FRONTEND=ON \
    -DENABLE_OV_IR_FRONTEND=ON \
    -DENABLE_OV_GENAI=ON
```

Compile:

```bash
make --jobs=$(nproc)
```

The build will now **statically link** `libjemalloc.so` into the OpenVINO runtime libraries.

---

## Step 7: Install OpenVINO into Virtual Environment

```bash
WHEEL_PATH=$(find . -name "openvino*.whl" | head -n 1)
pip install "$WHEEL_PATH"
```

Verify:

```bash
python -c "import openvino; print('OpenVINO version:', openvino.__version__)"
```

---

## Step 8: Install OpenVINO GenAI

```bash
pip install openvino-genai
```

Verify:

```bash
python -c "from openvino import genai; print('OpenVINO GenAI imported')"
```

> 🔗 [OpenVINO GenAI PyPI](https://pypi.org/project/openvino-genai/)

---

## Step 9: Verify jemalloc is Linked

Check if `jemalloc` symbols are embedded in the OpenVINO library:

```bash
# Find the core OpenVINO shared library
find venv/lib/python*/site-packages/openvino -name "*.so" -exec ldd {} \; | grep jemalloc
```

Should show:
```
libjemalloc.so.2 => /usr/lib/x86_64-linux-gnu/libjemalloc.so.2
```

Or use `objdump`:

```bash
objdump -p venv/lib/python*/site-packages/openvino/*.so | grep NEEDED | grep jemalloc
```

Should output:
```
NEEDED               libjemalloc.so.2
```

✅ This confirms `jemalloc` is **directly linked**, not just preloaded.

---

## Step 10: Install Agentic Coding Dependencies

```bash
pip install \
    optimum-intel \
    huggingface-hub \
    transformers \
    llama-index \
    llama-index-llms-openvino-genai \
    jupyter
```

---

## Troubleshooting

### 1. **jemalloc Not Found During Link**

Ensure `libjemalloc-dev` is installed:

```bash
dpkg -L libjemalloc-dev | grep libjemalloc.so
```

If not found, reinstall:
```bash
sudo apt install --reinstall libjemalloc-dev
```

### 2. **Linking Fails with "undefined reference"**

The patch assumes the library is named `libjemalloc.so`. Verify:

```bash
ls /usr/lib/x86_64-linux-gnu/libjemalloc*
```

If the name differs (e.g., `libjemalloc.so.2`), update the `find_library` call:

```cmake
find_library(JEMALLOC_LIB jemalloc.so.2 PATHS /usr/lib/x86_64-linux-gnu)
```

### 3. **oneMKL Not Used**

Check CMake output for:
```
-- MKL-DNN enabled
-- Intel Math Kernel Library: YES
```

Ensure `source /opt/intel/oneapi/setvars.sh` was run before CMake.

---

## Conclusion

You’ve successfully built **OpenVINO GenAI** with:

- ✅ **Intel oneMKL** via oneAPI for accelerated math
- ✅ **Ubuntu’s system jemalloc** linked directly via `-ljemalloc`
- ✅ No need for `LD_PRELOAD`—memory allocation is built-in
- ✅ Ready for **agentic coding** with optimized performance

This configuration ensures that **all OpenVINO-managed memory allocations** go through `jemalloc`, reducing fragmentation and improving concurrency in agent workloads.

> 📚 **Official Docs**:
> - [OpenVINO 2025 Installation](https://docs.openvino.ai/2025/get-started/install-openvino.html)
> - [OpenVINO GenAI GitHub](https://github.com/openvinotoolkit/openvino/tree/master/modules/genai)
> - [Optimum-Intel](https://huggingface.co/docs/optimum/intel/index)

Happy coding with your optimized AI agents!
