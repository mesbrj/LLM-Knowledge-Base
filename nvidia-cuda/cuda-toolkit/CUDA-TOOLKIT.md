## GPU Access in WSL2

***NVIDIA Driver installed only on Windows host***

- [CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html#getting-started-with-cuda-on-wsl-2) 
    - [CUDA Toolkit (WSL-Ubuntu)](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local)

![](/nvidia-cuda/cuda-toolkit/simpleMPI.png)

- [CUDA Samples](https://github.com/NVIDIA/cuda-samples)



```shell
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

git clone https://github.com/NVIDIA/cuda-samples.git

cd cuda-samples && mkdir build && cd build

# Install OpenGL/GLUT, Vulkan, FreeImage, OpenMP and Open MPI libraries to compile all CUDA samples
sudo apt install -y libgl1-mesa-dev libglu1-mesa-dev freeglut3-dev libglew-dev libglfw3-dev libfreeimage-dev libvulkan-dev vulkan-tools libopenmpi-dev openmpi-bin libegl1-mesa-dev

cmake .. && make -j$(nproc)

```
**NvSCI Sample not available to build and compile**. NVIDIA Software Communications Interface (NvSCI) is designed for:

- **Embedded platforms**: NVIDIA Jetson, NVIDIA DRIVE (automotive)
- **Safety-critical applications**: Inter-process communication with strict determinism
- **Hardware-specific features**: Direct memory sharing between different engines (CPU, GPU, DLA, etc.)
- **limitations**: Lacks the low-level hardware access NvSCI requires / No support for NvSCI's inter-engine synchronization primitives / Not included in standard desktop CUDA Toolkit distributions

**CUDA-OpenGL and CUDA-Vulkan interoperability** samples will compile but will not run in WSL2 due to lack of support with paravirtualized GPU.

**Samples that will run in WSL2**:

- CUDA computing
- OpenMP and Open MPI
- GPU image processing: FreeImage and NPP (NVIDIA Performance Primitives)


### Running samples:
- CUDA Runtime API, Vector Addition

![](/nvidia-cuda/cuda-toolkit/vectorAdd.png)

- This sample demonstrates how to use OpenMP API to write an application for multiple GPUs

![](/nvidia-cuda/cuda-toolkit/cudaOpenMP.png)
- Simple example demonstrating how to use MPI in combination with CUDA

![](/nvidia-cuda/cuda-toolkit/simpleMPI.png)

- This CUDA Sample demonstrates how to use NPP for histogram equalization for image data

![](/nvidia-cuda/cuda-toolkit/histEqualizationNPP.png)

## GPU Access in WSL2 containers
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)