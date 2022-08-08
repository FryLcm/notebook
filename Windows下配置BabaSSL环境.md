## 1、先从GitHub上下载源码到桌面

## 2、以管理员权限，打开VS Tools中“适用于VS 2017 的x64 本机工具命令提示”。

## 3、进入BabaSSL源码目录
```


```
## 4、配置编译生成64位的库
```
perl Configure VC-WIN64A
```

## 5、编译
```
nmake
```

## 6、安装
```
nmake install
```

## 7、配置环境变量
将BabaSSL命令行工具路径C:\Program Files\BabaSSL\bin，添加到环境变量中。

在cmd中，输入命令查看BabaSSL版本。
```
gmssl version
```

## 8、vs2017配置Babassl
1.项目 - 属性 - 平台选择x64
2.项目 - 属性 - VC++目录 - 包含目录 - 添加C:\Program Files (x86)\GmSSL\include
3.项目 - 属性 - VC++目录 - 库目录 - 添加C:\Program Files (x86)\GmSSL\lib
4.项目 - 属性 - 链接器 - 输入 - 附加依赖项 - 添加 libcrypto.lib、libssl.lib

## 9、样例


## 10、出现错误
若出现错误：
OPENSSL_Uplink(006E9000,08): no OPENSSL_Applink 

解决方法：
在自己的程序中，加上这样的源代码：
```
extern "C"
{
#include <openssl/applink.c>
}
```