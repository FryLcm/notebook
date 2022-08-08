

#  git工作流程

我们知道Git有三大区（**工作区、暂存区、版本库**）以及几个状态（**untracked、unstaged、uncommited**），下面只是简述下Git的大概工作流程，详细的可以参见其他有关Git的文章[【链接】。](http://www.cnblogs.com/cposture/category/642672.html)  

（1）打开你的项目文件夹，除了隐藏的.git文件夹，其他项目文件位于的地方便是工作区，工作区的文件需要添加到Git的暂存区（git add），随后再提交到Git的版本库（git commit）。

（2）首次新建的文件都是untracked状态（未跟踪），此时需要git  add到暂存区，Git便会在暂存区中生成一个该文件的索引，文件此时处于uncommited状态，需要git  commit生成版本库。添加到了版本库之后，再对文件进行修改，那么文件的状态会变为unstaged状态。

简单的认识了Git的工作流程，接下来便可以看看如何删除错误添加到暂存区或版本库里的文件了！



# 1 删除错误添加到暂存区的文件

有时你在工作区新建了文件TestFile，并且已经将它添加到了暂存区，git会告知，现有有一个文件未提交到版本库

### 1.1 仅仅删除暂存区里的文件    

​    此时你想撤销错误添加到暂存区里的文件，可以输入以下命令：

```
git rm --cache 文件名
```

 上面的命令仅仅删除暂存区的文件而已，不会影响工作区的文件，如上图，TestFile.txt仍然存在，此时输入下面命令，git会告知有一个未跟踪的文件TestFile.txt。

```
git status
```



### 1.2 删除暂存区和工作区的文件

```
git rm -f 文件名
```

**工作区的文件也被删除了。**

# 2 删除错误提交的commit

有时，不仅添加到了暂存区，而且commit到了版本库，这个时候就不能使用git rm了，需要使用git reset命令。

**错误提交到了版本库，此时无论工作区、暂存区，还是版本库，这三者的内容都是一样的**，所以在这种情况下，只是删除了工作区和暂存区的文件，下一次用该版本库回滚那个误添加的文件还会重新生成。

这个时候，我们必须撤销版本库的修改才能解决问题！

git reset有三个选项，--hard、--mixed、--soft。

```
//仅仅只是撤销已提交的版本库，不会修改暂存区和工作区
git reset --soft 版本库ID
```

 

```
//仅仅只是撤销已提交的版本库和暂存区，不会修改工作区
git reset --mixed 版本库ID
```

 

```
//彻底将工作区、暂存区和版本库记录恢复到指定的版本库
git reset --hard 版本库ID
```

​    那我们到底应该用哪个选项好呢？

​    （1）如果你是在提交了后，对工作区的代码做了修改，并且想保留这些修改，那么可以使用git reset --mixed 版本库ID，注意这个版本库ID应该不是你刚刚提交的版本库ID，而是**刚刚提交版本库的上一个版本库**。

​    （2）如果不想保留这些修改，可以直接使用彻底的恢复命令，git reset --hard 版本库ID。

​    （3）为什么不使用--soft呢，因为它只是恢复了版本库，**暂存区仍然存在你错误提交的文件索引**，还需要进一步使用上一节的删除错误添加到暂存区的文件，详细见上文。







# git命令分类整理

### 全局设置

    git config --global user.name xxx：设置全局用户名，信息记录在~/.gitconfig文件中
    git config --global user.email xxx@xxx.com：设置全局邮箱地址，信息记录在~/.gitconfig文件中
    git init：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中

### 常用命令

    git add XX ：将XX文件添加到暂存区
    git commit -m "给自己看的备注信息"：将暂存区的内容提交到当前分支
    git status：查看仓库状态 	--git status 命令用于查看在你上次提交之后是否有对文件进行再次修改。
    git log：查看当前分支的所有版本
    git push -u (第一次需要-u以后不需要) ：将当前分支推送到远程仓库
    git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下
    git branch：查看所有分支和当前所处分支

### 查看命令

    git diff XX：查看XX文件相对于暂存区修改了哪些内容
    git status：查看仓库状态
    git log：查看当前分支的所有版本
    git log --pretty=oneline：用一行来显示
    git reflog：查看HEAD指针的移动历史（包括被回滚的版本）
    git branch：查看所有分支和当前所处分支
    git pull ：将远程仓库的当前分支与本地仓库的当前分支合并

### 删除命令

    git rm --cached XX：将文件从仓库索引目录中删掉，不希望管理这个文件
    git restore --staged xx：==将xx从暂存区里移除==
    git checkout — XX或git restore XX：==将XX文件尚未加入暂存区的修改全部撤销==

### 代码回滚

    git reset --hard HEAD^ 或git reset --hard HEAD~ ：将代码库回滚到上一个版本
    git reset --hard HEAD^^：往上回滚两次，以此类推
    git reset --hard HEAD~100：往上回滚100个版本
    git reset --hard 版本号：回滚到某一特定版本

### 远程仓库

    git remote add origin git@git.acwing.com:xxx/XXX.git：将本地仓库关联到远程仓库
    git push -u (第一次需要-u以后不需要) ：将当前分支推送到远程仓库
    git push origin branch_name：将本地的某个分支推送到远程仓库
    git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下
    git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支
    git push -d origin branch_name：删除远程仓库的branch_name分支
    git checkout -t origin/branch_name 将远程的branch_name分支拉取到本地
    git pull ：将远程仓库的当前分支与本地仓库的当前分支合并
    git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并
    git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应

### 分支命令

    git branch branch_name：创建新分支
    git branch：查看所有分支和当前所处分支
    git checkout -b branch_name：创建并切换到branch_name这个分支
    git checkout branch_name：切换到branch_name这个分支
    git merge branch_name：将分支branch_name合并到当前分支上
    git branch -d branch_name：删除本地仓库的branch_name分支
    git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支
    git push -d origin branch_name：删除远程仓库的branch_name分支
    git checkout -t origin/branch_name 将远程的branch_name分支拉取到本地
    git pull ：将远程仓库的当前分支与本地仓库的当前分支合并
        git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并
    git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应

### stash暂存

    git stash：将工作区和暂存区中尚未提交的修改存入栈中
    git stash apply：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素
    git stash drop：删除栈顶存储的修改
    git stash pop：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素
    git stash list：查看栈中所有元素

git常用操作

git删除暂存区或版本库中的文件

# git一般步骤

`git config --global user.name xxx`：设置全局用户名，信息记录在~/.gitconfig文件中

`git config --global user.email xxx@xxx.com`：设置全局邮箱地址，信息记录在~/.gitconfig文件中

.gitconfig文件在家目录下，可以直接vim .gitconfig查看内容

先创建一个文件夹，再

`git init`：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中

这时可以在该文件夹下进行项目建设

`git status`：查看仓库状态，此时已修改的文件显示为红色，表示这些文件还未加入到暂存区中

```
git add XX：将XX文件添加到暂存区
git add .：将所有待加入暂存区的文件加入暂存区
```

：将修改后的文件加入到暂存区

此时再用`git status`，之前的红色文件会显示为绿色，表示现在已经加入到暂存区中。

`git commit -m "给自己看的备注信息"`：将暂存区的内容提交到当前分支

`git diff XX`：查看XX文件相对于暂存区修改了哪些内容，显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别。 

显示暂存区和上一次提交(commit)的差异: 

```
git diff --cached XX
或
git diff --staged XX
```

显示两次提交(commit)之间的差异: 

```
 git diff [first-branch]...[second-branch]
```

git删除暂存区中或已经commit到版本库中的文件，详情请阅读本文最上面。

```
git log：查看当前分支的所有版本

git log --pretty=oneline：用一行来显示
```

`git reflog`：查看HEAD指针的移动历史（包括被回滚的版本）

    git reset --hard HEAD^ 
    或
    git reset --hard HEAD~：将代码库回滚到上一个版本
    git reset --hard HEAD^^：往上回滚两次，以此类推
    git reset --hard HEAD~100：往上回滚100个版本
    git reset --hard 版本号：回滚到某一特定版本
    版本号可以用上面的git log来查看（前七位）
`git checkout — XX`或`git restore XX`：将XX文件尚未加入暂存区的修改全部撤销,就是将工作区的内容回滚到暂存区中，若此时暂存区无内容，则回滚到HEAD结点指向的版本。

ACgit设置中文：右上角，点击偏好设置，往下翻找到language，点击保存修改，over。

用ACgit之前得先添加ssh密钥，就是进入.ssh文件夹，找到id_rsa.pub，将里面的公钥复制到ACgit添加ssh密钥那里。若还未生成密钥公钥，可以到ssh登录那一章学习。

开始连接ACgit，并且将本地仓库推送到ACgit远程仓库上

```
git remote add origin git@git.acwing.com:xxx/XXX.git：将本地仓库关联到远程仓库

git push -u (第一次需要-u以后不需要)：将当前分支推送到远程仓库

git push origin branch_name：将本地的某个分支推送到远程仓库（acwing上的branch_name是master）

```

这时如果想把ACgit远程仓库中的文件克隆下来，可以

```
git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下

```

分支

```
git checkout -b branch_name：创建并切换到branch_name这个分支

git branch：查看所有分支和当前所处分支

git checkout branch_name：切换到branch_name这个分支

git merge branch_name：将分支branch_name合并到当前分支上
如果合并产生冲突，需要自己手动进入产生冲突的文件，将里面的内容按自己的想法进行更改，最后还需add和commit一下。

git branch -d branch_name：删除本地仓库的branch_name分支

git branch branch_name：创建新分支

```

更改完成后，需要git push同步到远程仓库。

创建了分支，需要将分支提交到云端，此时直接git push不行，因为云端上还没有这个分支，需要

```
git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支,
```

本地仓库的分支和远程仓库的分支具有一定的独立性，在本地删除分支在云端上不会删除，在云端上删除分支在本地上不会删除，需要自己手动删除。

```
git push -d origin branch_name：删除远程仓库的branch_name分支
```

这里还得去找资料学习一下！（1：33：10）

```
git pull：将远程仓库的当前分支与本地仓库的当前分支合并

git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并

git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应

git checkout -t origin/branch_name 将远程的branch_name分支拉取到本地
```



# 租云服务器



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





# docker一般步骤

docker包在下面这里

```
cd /var/lib/acwing
```

### 将当前用户添加到docker用户组

为了避免每次使用docker命令都需要加上sudo权限，可以将当前用户加入安装中自动创建的docker用户组：

```
sudo usermod -aG docker $USER

```

### 镜像（images）

    docker pull ubuntu:20.04：拉取一个镜像
    docker images：列出本地所有镜像
    docker image rm ubuntu:20.04 或 docker rmi ubuntu:20.04：删除镜像ubuntu:20.04
    docker [container] commit CONTAINER IMAGE_NAME:TAG：创建某个container的镜像
    docker save -o ubuntu_20_04.tar ubuntu:20.04：将镜像ubuntu:20.04导出到本地文件ubuntu_20_04.tar中
    docker load -i ubuntu_20_04.tar：将镜像ubuntu:20.04从本地文件ubuntu_20_04.tar中加载出来

### 容器(container)

    docker [container] create -it ubuntu:20.04：利用镜像ubuntu:20.04创建一个容器。
    docker ps -a：查看本地的所有容器
    docker ps：查看正在运行的所有容器
    docker [container] start CONTAINER：启动容器,这里包括下面的***CONTAINER***可以是容器的ID，也可以是名称。
    docker [container] stop CONTAINER：停止容器
    docker [container] restart CONTAINER：重启容器
    docker [contaienr] run -itd ubuntu:20.04：创建并启动一个容器
    	例：docker run -p 20000:22 -p 8000:8000 --name django_server -itd django_lesson:1.0
    docker [container] attach CONTAINER：进入容器
        先按Ctrl-p，再按Ctrl-q可以挂起容器
        Ctrl-d,是退出并关掉容器
    docker [container] exec CONTAINER COMMAND：在容器中执行命令
    docker [container] rm CONTAINER：删除容器
    docker container prune：删除所有已停止的容器
    docker export -o xxx.tar CONTAINER：将容器CONTAINER导出到本地文件xxx.tar中
    docker import xxx.tar image_name:tag：将本地文件xxx.tar导入成镜像，并将镜像命名为image_name:tag
    docker export/import与docker save/load的区别：
        export/import会丢弃历史记录和元数据信息，仅保存容器当时的快照状态
        save/load会保存完整记录，体积更大
    docker top CONTAINER：查看某个容器内的所有进程
    docker stats：查看所有容器的统计信息，包括CPU、内存、存储、网络等信息
    docker cp xxx CONTAINER:xxx 或 docker cp CONTAINER:xxx xxx：在本地和容器间复制文件
    docker rename CONTAINER1 CONTAINER2：重命名容器
    docker update CONTAINER --memory 500MB：修改容器限制















