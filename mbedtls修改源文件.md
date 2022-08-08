1. 修改源文件后，在mbedtls文件夹下找到visualc文件夹。
2. 在里面找到mbedTLS.sln并用visualstdio打开，右键项目点击重新生成（上面的win32还是64我也不知道要配哪个，都试试）。
3. 然后将生成的mbedTLS.lib文件复制到我们的项目中