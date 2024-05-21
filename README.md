# torchscript-cmake
How to load a TorchScript model in C++.

[Official Tutorial PyTorch](https://pytorch.org/tutorials/advanced/cpp_export.html#step-1-converting-your-pytorch-model-to-torch-script) - [YouTube](https://www.youtube.com/watch?v=RFq8HweBjHA&list=PLZAGo22la5t4UWx37MQDpXPFX3rTOGO3k)

## Fix bug
### 1. No CMAKE_CXX_COMPILER could be found

The "compiler" is a separate package that needs to be installed. One called g++ can be installed on it's own and is also included within a bundle of packages called "build-essential".

Thus ```sudo apt-get install build-essential``` solves the problem (and ```sudo apt-get install g++``` should also work), allowing ```cmake ..``` to work with no configuration necessary.
