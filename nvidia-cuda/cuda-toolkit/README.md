# GPU Access in WSL2

Pre-requisite:
- ***NVIDIA Driver installed only on Windows host***

## CUDA Toolkit in WSL2 - Ubuntu 24.04.3 LTS

- [CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html#getting-started-with-cuda-on-wsl-2) 
    - [CUDA Toolkit (WSL-Ubuntu)](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local)

![](/nvidia-cuda/cuda-toolkit/nvidia-smi.png)

- [CUDA Samples](https://github.com/NVIDIA/cuda-samples)


```shell
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

git clone https://github.com/NVIDIA/cuda-samples.git

cd cuda-samples && mkdir build && cd build

# Install OpenGL, Vulkan, FreeImage, OpenMP and Open MPI libraries and tools to compile all CUDA samples
sudo apt install -y libgl1-mesa-dev libglu1-mesa-dev freeglut3-dev libglew-dev libglfw3-dev libfreeimage-dev libvulkan-dev vulkan-tools libopenmpi-dev openmpi-bin libegl1-mesa-dev

cmake .. && make -j$(nproc)

```
## My experience with CUDA samples in WSL2

**NvSCI Sample not available to build and compile**. NVIDIA Software Communications Interface (NvSCI) is designed for:

- **Embedded platforms**: NVIDIA Jetson, NVIDIA DRIVE (automotive)
- **Safety-critical applications**: Inter-process communication (IPC) with strict determinism
- **Hardware-specific features**: Direct memory sharing between different engines (CPU, GPU, DLA, etc.)

**CUDA-OpenGL and CUDA-Vulkan interoperability** samples will compile but will not run in WSL2 due to lack of support with paravirtualized GPU.
- randomFog (build/Samples/4_CUDA_Libraries) is the only CUDA-OpenGL sample that runs successfully in WSL2.

**Samples that will run in WSL2**:
- CUDA computing
- OpenMP and Open MPI
- NPP (NVIDIA Performance Primitives) GPU image and signal processing


### Running samples:
- CUDA Runtime API, Vector Addition

![](/nvidia-cuda/cuda-toolkit/vectorAdd.png)

- This sample demonstrates how to use OpenMP API to write an application for multiple GPUs

![](/nvidia-cuda/cuda-toolkit/cudaOpenMP.png)
- Simple example demonstrating how to use MPI in combination with CUDA

![](/nvidia-cuda/cuda-toolkit/simpleMPI.png)

- This CUDA Sample demonstrates how to use NPP for histogram equalization for image data

![](/nvidia-cuda/cuda-toolkit/histEqualizationNPP.png)

- This sample illustrates pseudo- and quasi- random numbers produced by CURAND

![](/nvidia-cuda/cuda-toolkit/randomFog.png)

### Running (test) all samples
```shell
# from cuda-samples root directory
# Running all tests without JSON configuration file (test_args.json) for executables (--config)
python3 run_tests.py --output ./test --dir ./build/Samples
```
![](/nvidia-cuda/cuda-toolkit/run_tests.png)

***OBS:*** The `nbody` sample can run seccessfully with correct arguments like: `-benchmark -numbodies=512`. The failed tests need to be investigated individually and the JSON configuration file is necessary to run specific samples with arguments.

### Install samples
```shell
# cuda-samples root directory
cd build/
make install
# default install location:
# build/bin/x86_64/linux/release/
```