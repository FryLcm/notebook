

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
















