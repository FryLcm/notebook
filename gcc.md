## 编译过程

+ 1、预处理-Pre-Processing //.i文件
```
# -E 选项指示编译器仅对输入文件XXX进行预处理
gcc  -E XXX.c  -o XXX.i   //.i文件
```

+ 2、编译-Compiling // .s文件
```
# -S 编译选项告诉 gcc 在为 C++ 代码产生了汇编语言文件后停止编译
# gcc 产生的汇编语言文件的缺省扩展名是 .s 
gcc  -S XXX.i  -o XXX.s
```

+ 3、汇编-Assembling // .o文件
```
# -c 选项告诉 gcc 仅把源代码编译为机器语言的目标代码
# 汇编过程实际上指把汇编语言代码翻译成目标机器指令的过程。
# 缺省时 gcc 建立的目标代码文件有一个 .o 的扩展名。
gcc  -c XXX.s  -o XXX.o
```

+ 4、链接-Linking // bin文件
```
# -o 编译选项来为将产生的可执行文件用指定的文件名
gcc  XXX.o  -o XXX
```


## gcc重要编译参数
+ 1、-g 编译带调试信息的可执行文件
```
# -g 选项告诉 GCC 产生能被 GNU 调试器GDB使用的调试信息，以调试程序。
# 产生带调试信息的可执行文件XXX

gcc -g XXX.c -o XXX
```
+ 2、-O[n] 优化源代码
```
# 所谓优化，例如省略掉代码中从未使用过的变量、直接将常量表达式用结果值代替等等，
# 这些操作会缩减目标文件所包含的代码量，提高最终生成的可执行文件的运行效率。
# -O 选项告诉 g++ 对源代码进行基本优化。这些优化在大多数情况下都会使程序执行的更快。 
# -O2 选项告诉 g++ 产生尽可能小和尽可能快的代码。 如-O2，-O3，-On（n 常为0–3）
# -O 同时减小代码的长度和执行时间，其效果等价于-O1
# -O0 表示不做优化
# -O1 为默认优化
# -O2 除了完成-O1的优化之外，还进行一些额外的调整工作，如指令调整等。
# -O3 则包括循环展开和其他一些与处理特性相关的优化工作。
# 选项将使编译的速度比使用 -O 时慢， 但通常产生的代码执行速度会更快。
# 使用 -O2 优化源代码，并输出可执行文件

g++ -O2 XXX.cpp -o XXX
```
+ 3、**-l** 和 **-L** 指定库文件和指定库文件路径
```
# -l参数(小写L)就是用来指定程序要链接的库，-l参数紧接着就是库名
# 在/lib和/usr/lib和/usr/local/lib里的库直接用-l参数就能链接
# 链接XXX库 g++ -lXXX test.cpp -o test
# 如果库文件没放在上面三个目录里，需要使用-L参数(大写)指定库文件所在目录
# -L参数跟着的是库文件所在的目录名
# 链接mytest库，libmytest.so在/home/bing/mytestlibfolder目录下
g++ -L/home/bing/mytestlibfolder -lmytest test.cpp -o test
```
+ 4、-**I** 指定头文件搜索目录
```
# -I 
# /usr/include目录一般是不用指定的，gcc知道去那里找，
# 但是如果头文件不在/usr/icnclude里我们就要用-I参数指定了，
# 比如头文件放在/myinclude目录里，那编译命令行就要加上 -I/myinclude 参数了，
# 如果不加你会得到一个”xxxx.h: No such file or directory”的错误。
# -I参数可以用相对路径，比如头文件在当前 目录，可以用-I.来指定。

g++ -I/myinclude test.cpp -o test
```
+ 5、-**Wall** 打印警告信息
```
# 打印出gcc提供的警告信息

g++ -Wall test.cpp -o test
```
+ 6、-**w** 关闭警告信息
```
# 关闭所有警告信息

g++ -w test.cpp
```
+ 7、-std=c++11 设置编译标准
```
# 使用 c++11 标准编译 test.cpp

g++ -std=c++11 test.cpp
```
+ 8、-o 指定输出文件名
```
# 指定输出可执行文件名为test

g++ test.cpp -o test
```
+ 9、-D 定义宏
```
# 在使用gcc/g++编译的时候定义宏
# 常用场景：
# -DDEBUG 定义DEBUG宏，可能文件中有DEBUG宏部分的相关信息,
# 用个DDEBUG来选择开启或关闭DEBUG
```
+ 示例代码
```
// -Dname 定义宏name,默认定义内容为字符串“1”
#include <stdio.h>
int main()
{
    #ifdef DEBUG
   printf("DEBUG LOG\n");
    #endif
   printf("in\n");
}
// 1. 在编译的时候，使用gcc -DDEBUG main.cpp
// 2. 第七行代码可以被执行
```


## 生成库文件并编译
+ 1、文件目录
```
# 最初目录结构
.
├── include
│   └── Swap.h
├── main.cpp
└── src
   └── Swap.cpp
    
2 directories, 3 files
```
+ 2、链接静态库生成可执行文件
```
## 进入src目录下
$cd src

# 汇编，生成Swap.o文件
g++ Swap.cpp -c -I../include

# 生成静态库libSwap.a
ar rs libSwap.a Swap.o

## 回到上级目录
$cd ..

# 链接，生成可执行文件:static_main
g++ main.cpp -Iinclude -Lsrc -lSwap -o static_main
```
+ 3、链接动态库生成可执行文件
```
## 进入src目录下
$cd src

# 生成动态库libSwap.so
g++ Swap.cpp -I../include -fPIC -shared -o libSwap.so

## 上面命令等价于以下两条命令
# gcc Swap.cpp -I../include -c -fPIC
# gcc -shared -o libSwap.so Swap.o

## 回到上级目录
$cd ..

# 链接，生成可执行文件:share_main
g++ main.cpp -Iinclude -Lsrc -lSwap -o share_main
```
## 运行可执行文件
+ 1、运行静态库生成的可执行文件
```
./static_main
```
+ 2、运行动态库生成的可执行文件
```
## LD_LIBRARY_PATH是Linux环境变量名，
## 该环境变量主要用于指定查找共享库（动态链接库）时除了默认路径之外的其他路径。

LD_LIBRARY_PATH=src ./share_main
```

## server
```
gcc server.c -o server -L /usr/local/lib -lmbedcrypto -lmbedtls -lmbedx509
```

## client
```
gcc client.c -o client -L /usr/local/lib -lmbedcrypto -lmbedtls -lmbedx509
```

