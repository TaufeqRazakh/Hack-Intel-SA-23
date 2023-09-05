# Hack-Intel-SA-23
Using CMake to test libtorch with Intel pytorch extension

## So Where do I begin?
By looking at the available [Documentation](https://github.com/TaufeqRazakh/Hack-Intel-SA-23) on how to get started on Intel extension for PyTorch, you will notice an elegant example code which alludes to a very meaningful optimization. Our Objective is to build this starter code and make it run on a sample architecture the CMake way.

## How do I use CMake to build the starter code?
Specify a cmake compiler, a path where the libtorch shared object files are for linking. For this you will need to give the following information to CMake

    - `LIBTORCH_PREFIX` - This is where the installation of the PyTorch C++ SDK is with the header and shared files. This is aso where the libintel-ext-pyt files are.
    - `IPEX_INSTALL_PREFIX` - If this is already abaiable in your envoronment this means the system administrator has placed a path for the correct libtorch and libintel-ext-pt for CMake to use. If not specified, the `LIBTORCH_PREFIX` will be used.
    - `CMAKE_CXX_COMPILER` - We will want to use `icpx` from oneapi since we want to optimize the D2H and H2D tensor transfer with sycl.
    - Pass `-fsycl` to the `CMAKE_CXX_FLAGS`.

## What is this big optimization you talk about?
Since we wish to bring performance to the application from the tensor from torch on our compute node that has intel, it makes the most sense to transfer the PyTorch tensor to the GPU for some compute intensive kernel though in an optimized way. With SYCL support and possibility to generate byte code for the accelerator, this means we would be able to transfer the Tensor data to a device using a SYCL queue.  