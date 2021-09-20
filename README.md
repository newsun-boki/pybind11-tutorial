# Use pybind11 by cmake

帮助你在python中使用cmake中的函数

## step1

```bash
pip install pytest
```

## step2

你必须要把pybind11和CMakeLists.txt放到一个文件下。pybind11是在github下载的官方仓库

```bash
git clone https://github.com/pybind/pybind11.git
```

这里我已经下好了，你需要做的是在pybind11下

```bash
mkdir build
cd build
cmake ..
cmake --build . --config Release --target check
```

## step3
使用这个简单的官方代码example.cpp

```c++
#include <pybind11/pybind11.h>
namespace py = pybind11;
int add(int i, int j) {
    return i + j;
}

PYBIND11_MODULE(example, m) {
    m.doc() = "pybind11 example plugin"; // optional module docstring

    m.def("add", &add, "A function which adds two numbers");
}
```

CMakeLists也很简单

```CMakeLists
cmake_minimum_required(VERSION 2.8.12)
project(example)   

add_subdirectory(pybind11)
pybind11_add_module(example example.cpp)
```

## step4

cmake编译这个东西

```bash
mkdir build
cd build
cmake ..
make
```

会生成一个很长的.so文件，这个文件就是能够在python中调用c++的关键。
当你使用python调用c++函数时，请确保.so文件与python文件在同一目录下。

## Test

```python
import example
example.add(3, 4)
[out]: 7
```
