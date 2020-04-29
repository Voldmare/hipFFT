# hipFFT
hipFFT is a FFT marshalling library, with multiple supported backends.  It sits between the application and a 'worker' FFT library, marshalling inputs into the backend library and marshalling results back to the application.  hipFFT exports an interface that does not require the client to change, regardless of the chosen backend.  Currently, hipFFT supports [rocFFT](https://github.com/ROCmSoftwarePlatform/rocFFT) and [cuFFT](https://developer.nvidia.com/cufft) as backends.

## Installing pre-built packages
Download pre-built packages either from [ROCm's package servers](https://rocm.github.io/install.html#installing-from-amd-rocm-repositories) or by clicking the github releases tab and manually downloading, which could be newer(source only).  Release notes are available for each release on the releases tab.
* On ubuntu, `sudo apt update && sudo apt install hipfft`

## Building from source
### Library build dependency
* hipFFT library build depends on rocFFT on AMD platform or cuFFT on NVIDIA platform.
### Clients build dependency
* hipFFT clients build depend on FFTW, gtest, boost program-options.

`mkdir build && cd build`
`cmake -LH ..`  shows all build options, in which include specific options for this project with prefix "BUILD_*".

hipFFT build supports stardard compiler such as g++, clang, etc, as well as hipcc with various backends.

Here are cmake build examples under various scenarios:

| Hardware target |                     Case                  |                                 Build command line                                   |
| --------------- | ----------------------------------------- | ------------------------------------------------------------------------------------ |
|     AMD GPU     |  Build a project using HIP language APIs + hipFFT with standard host compiler | cmake -DCMAKE_CXX_COMPILER=g++ -DCMAKE_BUILD_TYPE=Release -DBUILD_CLIENTS_TESTS=ON -DBUILD_CLIENTS_SAMPLES=ON -L .. |
|     AMD GPU     |  Build a project using HIP language APIs + hipFFT + device kernels with HIP-hcc| HIP_PLAFORM=hcc cmake  -DCMAKE_CXX_COMPILER=/opt/rocm/bin/hipccDCMAKE_BUILD_TYPE=Release -DBUILD_CLIENTS_TESTS=ON -DBUILD_CLIENTS_SAMPLES=ON -L .. |
|     AMD GPU     |  Build a project using HIP language APIs + hipFFT + device kernels with HIP-clang | HIP_PLAFORM=hcc HIP_COMPILER=clang  cmake  -DCMAKE_CXX_COMPILER=/opt/rocm/bin/hipccDCMAKE_BUILD_TYPE=Release -DBUILD_CLIENTS_TESTS=ON -DBUILD_CLIENTS_SAMPLES=ON -L .. |
|  NVIDIA GPU     |  Build a project using HIP language APIs + hipFFT with standard host compiler | cmake -DCMAKE_CXX_COMPILER=g++  -DCMAKE_BUILD_TYPE=Release -DBUILD_CLIENTS_TESTS=ON -DBUILD_CLIENTS_SAMPLES=ON -DBUILD_WITH_LIB=CUDA -L .. |
|  NVIDIA GPU     |  Build a project using HIP language APIs + hipFFT + device kernels with HIP-nvcc | HIP_PLAFORM=nvcc cmake  -DCMAKE_CXX_COMPILER=/opt/rocm/bin/hipcc  -DCMAKE_BUILD_TYPE=Release -DBUILD_CLIENTS_TESTS=ON -DBUILD_CLIENTS_SAMPLES=ON -L .. |

## Quick CUDA porting guide
* [Hipfy](https://github.com/ROCm-Developer-Tools/HIP/blob/master/hipify-clang) your code and fix all unspported CUDA features or user-defined macros.
* Build it with HIP-nvcc and run on NVIDIA device.
* Build it with HIP-clang and run on AMD device.

See more details [here](https://rocm-documentation.readthedocs.io/en/latest/Programming_Guides/HIP-porting-guide.html "here")

## Functions supported
A list of [exported functions](https://github.com/ROCmSoftwarePlatform/hipFFT/wiki/Exported-functions) from hipfft can be found on the wiki

hipFFT does not support hcc build directly, and HIP-hcc build will be deprecated soon. 

