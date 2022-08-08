



## vs2017配置gmssl
1.项目 - 属性 - 平台选择x64
2.项目 - 属性 - VC++目录 - 包含目录 - 添加C:\Program Files (x86)\GmSSL\include
3.项目 - 属性 - VC++目录 - 库目录 - 添加C:\Program Files (x86)\GmSSL\lib
4.项目 - 属性 - 链接器 - 输入 - 附加依赖项 - 添加 libcrypto.lib、libssl.lib

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