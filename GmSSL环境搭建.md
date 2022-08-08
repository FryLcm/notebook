
## 1、先从GitHub上clone下来
```
git clone https://github.com/guanzhi/GmSSL.git

```

## 2、编译
GmSSL 3.0 采用了cmake构建系统。下载源代码后将其解压缩，进入源码目录，执行：
```
mkdir build
cd build
cmake ..
make
make test
sudo make install
```
## 3、测试

## 4、CMakeLists.txt的例子
```
cmake_minimum_required(VERSION 3.0)
project(sm2demo)

include_directories(/usr/local/include)
link_directories(/usr/local/lib)

add_executable(sm2keyparse sm2keyparse.c)
target_link_libraries(sm2keyparse gmssl)
```