# torchscript-cmake
How to load a TorchScript model in C++.

[Official Tutorial PyTorch](https://pytorch.org/tutorials/advanced/cpp_export.html#step-1-converting-your-pytorch-model-to-torch-script) - [YouTube](https://www.youtube.com/watch?v=RFq8HweBjHA&list=PLZAGo22la5t4UWx37MQDpXPFX3rTOGO3k)

## Fix bug
### 1. No CMAKE_CXX_COMPILER could be found

The "compiler" is a separate package that needs to be installed. One called g++ can be installed on it's own and is also included within a bundle of packages called "build-essential".

Thus ```sudo apt-get install build-essential``` solves the problem (and ```sudo apt-get install g++``` should also work), allowing ```cmake ..``` to work with no configuration necessary.

### 2. <torch/torch.h> not found

The solution was discussed here: [Where to find <torch/torch.h>?](https://discuss.pytorch.org/t/where-to-find-torch-torch-h/59908)

**Solution**: Another CMakeLists.txt from here: [Installing C++ Distributions of PyTorch](https://pytorch.org/cppdocs/installing.html).

```
cmake_minimum_required(VERSION 3.18 FATAL_ERROR)
project(example-app)

find_package(Torch REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

add_executable(example-app example-app.cpp)
target_link_libraries(example-app "${TORCH_LIBRARIES}")
set_property(TARGET example-app PROPERTY CXX_STANDARD 17)

# The following code block is suggested to be used on Windows.
# According to https://github.com/pytorch/pytorch/issues/25457,
# the DLLs need to be copied to avoid memory errors.
if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET example-app
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:example-app>)
endif (MSVC)
```
