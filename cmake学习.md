## 1 语法特性介绍

+ 基本语法格式：指令(参数 1 参数 2...) 

  + 参数使用括弧括起

  + 参数之间使用空格或分号分开 
+ 指令是大小写无关的，参数和变量是大小写相关的 
```
set(HELLO hello.cpp)
add_executable(hello main.cpp hello.cpp)
ADD_EXECUTABLE(hello main.cpp ${HELLO})
```

+ 变量使用${}方式取值，但是在 IF 控制语句中是直接使用变量名



## 2 重要指令

+ **cmake_minimum_required** - 指定CMake的最小版本要求
```
# CMake最小版本要求为3.0
cmake_minimum_required(VERSION 3.0)
```
+ **project** - 定义工程名称，并可指定工程支持的语言 
```
# 指定工程名为HELLOWORLD
project(HELLOWORLD)
```
+ **set** - 显式的定义变量
```
# 定义SRC变量，其值为sayhello.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```
+ **include_directories** - 向工程添加多个特定的头文件搜索路径 --->相当于指定g++编译器的-I参数
```
# 将/usr/include/myincludefolder 和 ./include 添加到头文件搜索路径
include_directories(/usr/include/myincludefolder ./include)
```
+ **link_directories** - 向工程添加多个特定的库文件搜索路径 --->相当于指定g++编译器的-L参数
```
# 将/usr/lib/mylibfolder 和 ./lib 添加到库文件搜索路径
link_directories(/usr/lib/mylibfolder ./lib)
```
+ **add_library** - 生成库文件
```
# 通过变量 SRC 生成 libhello.so 共享库
add_library(hello SHARED ${SRC})

# 通过变量 SRC 生成 libhello.a 静态库
add_library(hello STATIC ${SRC})
```
+ **add_compile_options** - 添加编译参数
```
# 添加编译参数 -Wall -std=c++11 -O2
add_compile_options(-Wall -std=c++11 -O2)
```
+ **add_executable** - 生成可执行文件
```
# 编译main.cpp生成可执行文件main
add_executable(main main.cpp)
```
+ **target_link_libraries** - 为 target 添加需要链接的共享库 --->相同于指定g++编译器-l参数
```
# 将hello动态库文件链接到可执行文件main
target_link_libraries(main hello)
```
+ **add_subdirectory** - 向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置
```
# 添加src子目录，src中需有一个CMakeLists.txt
add_subdirectory(src)
```
+ **aux_source_directory** - 发现一个目录下所有的源代码文件并将列表存储在一个变量中，这个指令临时被用来自动构建源文件列表
```
# 定义SRC变量，其值为当前目录下所有的源代码文件
aux_source_directory(. SRC)
# 编译SRC变量所代表的源代码文件，生成main可执行文件
add_executable(main ${SRC})
```



+ **CMAKE_C_FLAGS** **gcc**编译选项

+ **CMAKE_CXX_FLAGS** **g++**编译选项

```
# 在CMAKE_CXX_FLAGS编译选项后追加-std=c++11
set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
```

+ **CMAKE_BUILD_TYPE** **编译类型**(Debug, Release)

```
# 设定编译类型为debug，调试时需要选择debug
set(CMAKE_BUILD_TYPE Debug)
# 设定编译类型为release，发布时需要选择release
set(CMAKE_BUILD_TYPE Release)
```



+ **CMAKE_C_COMPILER**：指定**C**编译器

+ **CMAKE_CXX_COMPILER**：指定**C++**编译器

+ **EXECUTABLE_OUTPUT_PATH**：可执行文件输出的存放路径

+ **LIBRARY_OUTPUT_PATH**：库文件输出的存放路径



## 3 CMake编译工程



```
## 外部构建
# 1. 在当前目录下，创建build文件夹
mkdir build 
# 2. 进入到build文件夹
cd build
# 3. 编译上级目录的CMakeLists.txt，生成Makefile和其他文件
cmake ..
# 4. 执行make命令，生成target
make
```



## 4 示例

```
#设置最低版本
cmake_minimum_required(VERSION 3.0)

#设置名称
project(HELLOWORLD)

#
add_executable(test test.c)

#
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR})

```

