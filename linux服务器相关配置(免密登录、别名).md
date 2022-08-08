登录
ssh ali_zzx@101.200.180.233
zhongzx3578264

ssh root@101.200.180.233
Zhongzx3578264


1、登录服务器
```
ssh root@101.200.180.233

输入密码：zhongzx3578264
```


2、设置服务器别名
```

```


3、设置免密登录
```
ssh-copy-id 服务器别名
```


4、端口设置
```
我在容器django中设置了20001 和8000

我在容器docker中设置了20000
```

### 登录到新服务器

```
ssh root@xxx.xxx.xxx.xxx  # xxx.xxx.xxx.xxx替换成新服务器的公网IP
```

### 创建acs用户

```
adduser acs  # 创建用户acs

usermod -aG sudo acs  # 给用户acs分配sudo权限

```

### 配置免密登录方式

#### 配置文件

创建文件` ~/.ssh/config`。

然后在文件中输入：

```
Host myserver1
    HostName IP地址或域名
    User 用户名

Host myserver2
    HostName IP地址或域名
    User 用户名
```

之后再使用服务器时，可以直接使用别名myserver1、myserver2。

#### 密钥登录

创建密钥：

```
ssh-keygen
```

然后一直回车即可。

执行结束后，`~/.ssh/`目录下会多两个文件：

    id_rsa：私钥
    id_rsa.pub：公钥
之后想免密码登录哪个服务器，就将公钥传给哪个服务器即可。

例如，想免密登录myserver服务器。则将公钥中的内容，复制到myserver中的~/.ssh/authorized_keys文件里即可。

也可以使用如下命令一键添加公钥：

```
ssh-copy-id myserver
```



### 配置新服务器的工作环境

将AC Terminal的配置传到新服务器上：

```
scp .bashrc .vimrc .tmux.conf server_name:  # server_name需要换成自己配置的别名
```

### 安装tmux和docker

登录自己的服务器，然后安装`tmux`：

```
sudo apt-get update 每次配置环境前运行一下

sudo apt-get install tmux
```

打开tmux。（养成好习惯，所有工作都在tmux里进行，防止意外关闭终端后，工作进度丢失）

然后在tmux中根据docker安装教程安装docker即可。



# 在云服务器中安装docker

第一步

```
 sudo apt-get update

 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

第二步

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

第三步

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

第四步

```
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

```

第五步

```
docker --version  看docker版本号，基本上能看到版本号就说明安装好了

```

此时云服务器基本配置完成。可以再`sudo apt-get install tree `,以后就可以用tree命令。

如果不小心把云服务器玩坏了，可以点击实例，先停止这个实例，再找到更换操作系统，就是重装系统。

