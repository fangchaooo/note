# CMAKE使用

### 快速开始

C++ file:

```c++
//helloworld.cppadd_executable
#include<iostream>

int main(int argc, char *argv[]){
   std::cout << "Hello World!" << std::endl;
   return 0;
}
```

CMakeLists.txt file:

```cmake
cmake_minimum_required(VERSION 2.8.9)
project (hello)
add_executable(hello helloworld.cpp)
```

- 第一行是本机的cmake的版本号
- 第二行时对这个project的命名，使用了`project()`命令
- 第三行是`add_executable()`命令，它请求使用`helloworld.cpp`源文件构建可执行文件。`add_executable()`函数的第一个参数是要构建的可执行文件的名称，第二个参数是构建可执行文件的源文件。

### 有目录项目

```bash
|-- CMakeLists.txt
 |-- build
 |-- include
 |   \-- Student.h
 \-- src
     |-- Student.cpp
     \-- mainapp.cpp
```

```cmake
cmake_minimum_required(VERSION 2.8.9)
project(directory_test)

include_directories(include)

#set(SOURCES src/mainapp.cpp src/Student.cpp)
file(GLOB SOURCES "src/*.cpp")

add_executable(testStudent ${SOURCES})
```

- include_directories 命令是整个项目的头文件
- set命令是逐个添加你的cpp文件
- 使用file 命令可以简化set命令，GLOB创建所有满足globbing表达式的文件列表，并添加到变量SOURCES中

### 构建共享库(.so)

```bash
|-- CMakeLists.txt
 |-- build
 |-- include
 |   \-- Student.h
 \-- src
 .   \-- Student.cpp
```

```cmake
cmake_minimum_required(VERSION 2.8.9)
project(directory_test)

set(CMAKE_BUILD_TYPE Release)
 
include_directories(include)
file(GLOB SOURCES "src/*.cpp")

add_library(testStudent SHARED ${SOURCES})

install(TARGETS testStudent DESTINATION /usr/lib)
```

- set用于将构建版本类型设置为发布版本
- add_library，该库使用`SHARED`标志构建为共享库（其他选项为：STATIC或MODULE），**testStudent**名称用作共享库的名称。
- 最后一行使用该`install()`函数定义库的安装位置（在本例中为/usr/lib）。在这种情况下，使用对**sudo make install**的调用来调用部署。

### 构建静态库(.a)

静态库实际就是一堆.so的打包

使用gcc的ar命令

`ar -t xxx.a`，解包出来的就是一堆.so文件

```cmake
cmake_minimum_required(VERSION 2.8.9)
project(directory_test)
set(CMAKE_BUILD_TYPE Release)

include_directories(include)

file(GLOB SOURCES "src/*.cpp")
 
add_library(testStudent STATIC ${SOURCES})
 
install(TARGETS testStudent DESTINATION /usr/lib)
```

### 使用共享库或静态库

```cmake
cmake_minimum_required(VERSION 2.8.9)
project (TestLibrary)
 
# 链接共享库
set ( PROJECT_LINK_LIBS libtestStudent.so )
link_directories( ~/exploringBB/extras/cmake/studentlib_shared/build )
 
#链接静态库
#set ( PROJECT_LINK_LIBS libtestStudent.a )
#link_directories( ~/exploringBB/extras/cmake/studentlib_static/build )
 
include_directories(~/exploringBB/extras/cmake/studentlib_shared/include)
 
add_executable(libtest libtest.cpp)
target_link_libraries(libtest ${PROJECT_LINK_LIBS} )
```

