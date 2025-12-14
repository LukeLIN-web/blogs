
# docker



[github have a projcet:docker_mirror](https://github.com/silenceshell/docker_mirror) could auto test which source is the fastest in your network environment

登录阿里云, 镜像源https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors 左边的**镜像加速器**. 

[MIT_6.828 环境配置_helloworld19970916的博客-CSDN博客](https://blog.csdn.net/helloworld19970916/article/details/81603056)

卡到setting都打不开,绝了.重启, 关闭, 多试几次好像就变流畅了

你会发现和网上很多图不一样,因为

"网络"选项卡等各种选项在 Windows 容器模式下不可用，因为网络CPU 由 Windows 管理。不同的设置可用于配置，具体取决于您是使用 WSL 2 模式下的 Linux 容器、Hyper-V 模式下的 Linux 容器还是 Windows 容器。

#### 下载到哪里呢?

windows会下载到 C:\users\Public\Documents公用文档中.您的卷目录是`/var/lib/docker/volumes/blog_postgres-data/_data`，`/var/lib/docker`通常安装在`C:\Users\Public\Documents\Hyper-V\Virtual hard disks`。

## 命令

### build  

-t  

### run

创建一个新的容器并运行一个命令

```bash
-name 
-d detach模式,在后台运行
-p 80:80  把host 80 端口映射到contain 80 端口
-i  interactive 交互式
-t 终端
-e username="ritchie": 设置环境变量；
-v /data:/data #主机的目录 /data 映射到容器的 /data。
--rm 在容器终止运行后自动删除容器文件。
```

```bash
run一个image
docker run -i ucbbar/riscv-docker-images:b266e97-3_922-1.0.1 /bin/bash 但是下一次start不起来. 不能start /bin/bash
container
$ docker container run hello-world
$ docker container run -it ubuntu bash #新建一个ubuntu容器,进bash如果要退出就：Ctrl-D 或者exit
docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash #容器的 3000 端口映射到本机的 8000 端口。 这里可能要开一下防火墙, Node 进程运行在 Docker 容器的虚拟环境里面，进程接触到的文件系统和网络接口都是虚拟的，与本机的文件系统和网络接口是隔离的，因此需要定义容器与物理机的端口映射（map)
docker 怎么下一次run还可以用上次的文件

```

### 挂载绑定

```
docker run -t -i -v /d/PycharmProjects:/test ldzm/myubuntu:14.04 /bin/bash
```

-v /d/PycharmProjects:/test
-v挂载本地文件夹到docker容器中，在容器中修改/test文件夹中的内容也就是修改D:\PycharmProjects文件夹中的内容
/d/PycharmProjects对应的windows的文件夹路径为D:\PycharmProjects
/test为容器中的文件的绝对路径

```bash
docker run -itd -v /d/PycharmProjects:/work  --name try1 kortanzh/xv6 bash #try1是容器的名字. 会新建D:\PycharmProjects
docker run -itd -v /d/PycharmProjects:/work -e BUILDER_UID=123 -e BUILDER_GID=456 --name container_name kortanzh/xv6 bash
```

-v /d/PycharmProjects:/work
-v挂载本地文件夹到docker容器中，在容器中修改/work文件夹中的内容也就是修改D:\PycharmProjects文件夹中的内容
/d/PycharmProjects对应的windows的文件夹路径为D:\PycharmProjects
/work为容器中的文件的绝对路径

首先docker容器的Linux对Windows支持并不是很高，他只对C:\Users 目录下进行挂载，其他目录都没有办法挂载，除非用VirtualBox修改这个虚拟机的共享目录设定，否则在虚拟机里只能看到C:\Users以下的文件

```bash
docker run -d  -v /c/Users/systemDir:/usr/local/log balance
```

### start

```
docker start -a #{containerName}/#{containerID} 但是开一下就关了.怎么让他一直运行呢?
```



### exec

 在运行的容器中执行命令,退出container时，让container仍然在后台运行

```bash
docker exec -it  --user "$(id -u)" mynginx /bin/sh /root/runoob.sh #在容器 mynginx 中以交互模式执行容器内 /root/runoob.sh 脚本
docker exec -it 9df70f9a0714 /bin/bash  #通过 exec 命令对指定id的容器执行 bash:
docker exec -i -t 5c72d3b508ed /bin/bash #打开已经在运行的container终端 , 可以简化为 -it 
```

暂停容器

有时我们只是希望让容器暂停工作一段时间，比如要对容器的文件系统打个快照，或者docker host需要使用CPU，这是执行docker pause。

处于暂停状态的容器不会占用CPU资源，直接通过docker unpause恢复运行。

#### 为什么要用非root用户启动容器

默认情况下，容器中的进程以 root 用户权限运行，并且这个 root 用户和宿主机中的 root 是同一个用户。所以大家直接启动容器，并在容器内部跑程序时，你在宿主机上用top等命令查看进程时，以及用nvidia-smi查看显卡使用时，显示的都是root用户在运行。

这样会带来几个问题。1. 你在容器中保存的文件的拥有者并不是你，而是root用户，当你的容器被销毁后，你在宿主机上是没有权限对这些文件进行操作的。2. 对于其他用户来说，他人无法通过nvidia-smi查看显卡是谁在使用，因为通过进程ID查到的是root用户在跑程序。这不方便同事之间进行显卡利用的沟通，也不方便管理员监管。当部分进程有问题时，管理员不知道找谁。

#### 怎么以非root用户启动容器。

```shell
docker run --user $(id -u ${USER}):$(id -g ${USER})  <其他参数>
docker run -itd -v /path/to/workfolder:/work -e BUILDER_UID="$(id -u)" -e BUILDER_GID="$(id -g)" --name container_name kortanzh/xv6 bash #linux 是BUILDER_UID="$(id -u)" -e BUILDER_GID="$(id -g)" ,windows是  --user $(id -u ${USER}):$(id -g ${USER})
```

通过 `--user $(id -u ${USER}):$(id -g ${USER})` 的参数可以指定以当前宿主机用户的身份启动容器。`–-user`是用来指定docker容器中用户的id的，`$(id -u ${USER}):$(id -g ${USER})` 是自动解析id命令返回的uid和组id，这样就不用自己去查询id了。

所以相比于之间大家使用docker，唯一的改变就是在启动容器的时候加上`--user $(id -u ${USER}):$(id -g ${USER})`这一段话就可以了。

### attach

连接到正在运行中的容器。**Do not use `exit` to quit if you are half done your jobs, use `ctrl-p ctrl-q` to detach from the container.**  CTRL-C不仅会导致退出容器，而且还stop了

[vscode远程连接docker容器__这也太刺激了吧的博客-CSDN博客](https://blog.csdn.net/weixin_40641725/article/details/105512106)

上面的方法用vscode直接连接而不是用命令行. gui很爽.

**docker logs :** 获取容器的日志

### 复制docker

```dockerfile
先mkdir, docker ps看一下名字
docker cp silly_rosalind:/root/file.c windoc 从docker到windows的windoc文件夹
docker cp windoc silly_rosalind:/root/a.out  从windows复制到docker
```



## container文件

```bash
docker container run #命令会从 image 文件，生成一个正在运行的容器实例container。
docker ps -a #看所有的container，包括运行中的，以及未运行的或者说是沉睡镜像
docker attach container_name   #container运行在后台，如果想进入它的终端,用attach有一个缺点，那就是每次从container中退出到前台时，container也跟着结束任务了。
docker container rm goofy_almeida#删除运行的容器文件，释放硬盘空间
docker container kill [containID] #stop可以过一会儿停. kill是马上停
docker container start  [containID] #重复使用容器，它用来启动已经生成、已经停止运行的容器文件。
```

注意，`docker container run`命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的`docker image pull`命令并不是必需的步骤。



## image文件

```bash
# 列出本机的所有 image 文件。
$ docker image ls

# 删除 image 文件
$ docker image rm [imageName]

$ docker image pull library/hello-world
```

**Docker 把应用程序及其依赖，打包在 image 文件里面。**只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。

为了方便共享，image 文件制作完成后，可以上传到网上的仓库。此外，出售自己制作的 image 文件也是可以的。



#### 自己制作 image 文件

1. 源码

2. 写dockerfile 文件 

   ```dockerfile
   FROM node:8.4
   COPY . /app #不用taobao源我用了180秒,用了后用了10秒
   WORKDIR /app
   RUN npm install --registry=https://registry.npm.taobao.org #不用淘宝源就会npm链接不上 ,用了淘宝源后用了30秒
   EXPOSE 3000
   CMD node demos/01.js #一个 Dockerfile 可以包含多个RUN命令，但是只能有一个CMD命令。不能附加命令了（比如前面的/bin/bash），否则它会覆盖CMD命令
   ```

3. dockerignore文件 路径要排除，不要打包进入 image 文件.

```bash
docker image build -t koa-demo:0.0.1 .
```

上面代码中，`-t`参数用来指定 image 文件的名字，后面还可以用冒号指定标签。如果不指定，默认的标签就是`latest`。最后的那个点表示 Dockerfile 文件所在的路径，上例是当前路径，所以是一个点。

```bash
docker login
$ docker image tag [imageName] [username]/[repository]:[tag]
# 实例
$ docker image tag koa-demos:0.0.1 luke/koa-demo:0.0.1
也可以不标注用户名，重新构建一下 image 文件。
$ docker image build -t [username]/[repository]:[tag] .
$ docker image push [username]/[repository]:[tag]
# 实例 比如你注册的用户名叫luke,那就把repository改了
$ docker image push luke/koa-demo:0.0.1
```

解决docker push镜像时denied: requested access to the resource is denied 

### 登录

```bash
docker login
```

## 问题

1-5是ubuntu的问题 1 E: Unable to locate package git

解决方法:

```bash
apt-get update
apt-get install git
```

2 bash: sudo: command not found

解决办法 

```
apt-get update
apt-get install sudo
```

3 下载很慢,更换阿里源:vi /etc/apt/sources.list

bash: vi: command not found

解决方法:

```bash
cp sources.list source.list_back #先备份因为用阿里源可能会Unable to correct problems, you have held broken packages.
rm sources.list 
echo "deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse" >> sources.list
```

将原有内容删除，并替换为以下的阿里源

``` bash
   deb http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe
   deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe
```

4 递归安装, 发现啥都没有装, 由于Docker镜像站中的Ubuntu镜像是一个最小版本，连vi也没有:( 每次创建容器时都需要更换国内源和安装gcc等工具E: Unable to correct problems, you have held broken packages.

可能是因为中止安装后更换了安装源导致的,但是我没有更换源也不行.

解决方法: 

https://askubuntu.com/questions/140246/how-do-i-resolve-unmet-dependencies-after-adding-a-ppa

 1运行apt-get -f install来修复问题 

不行

2 

```bash
apt-get clean
apt-get autoclean
```

还是不行

搞不懂, 于是我卸载了ubuntu, 发现 居然有配好的6.S081 环境!! docker hub有点厉害的

5**错误：**
安装Ubuntu子系统后，更新资源时遇到了这个问题Release file for http://security.ubuntu.com/ubuntu/dists/focal-security/InRelease is not valid yet (invalid for another 6h 46min 29s). Updates for this repository will not be applied.

**解决方法：**
这个问题是因为时间不匹配导致的，由于我电脑的双系统切换会导致Windows下的时间没有同步，最初没在意这个问题导致了上面的报错。

windows同步了, container里面时间还是错的. 

**6 Error** response from daemon: open \.\pipe\docker_engine_linux: The system cannot find the file specified

解决方法: 

在win10 命令行提示符执行：

Net stop com.docker.service
Net start com.docker.service

7 windows没有uid和组id, 怎么--user启动? 

方法一: 我试试自己创建一个

```
docker run -itd -v /d/PycharmProjects:/work -e BUILDER_UID=123 -e BUILDER_GID=456 --name container_name kortanzh/xv6 bash  
返回 : 2fe47549fa9131f569a409e8f29c03a929ac1212e35bee0d68a5ad70411742ca
我再 docker exec -it --user 123 container_name bash 这个可以,
显示[adam@2fe47549fa91 work]$ 我怀疑他创建了一个user为123的.哈哈哈哈哈尝试成功了!
```

问题8 :WSL 2 installation is incomplete.

解决方法:

如果使用的是 ARM64 计算机，请下载[ARM64 包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi)。如果不确定自己计算机的类型，请打开命令提示符或 PowerShell，并输入：systeminfo | find "System Type"。

我们是中文的,所以这样是找不到的, 应该systeminfo 然后肉眼找 "系统类型". 

###  用docker来运行sql server

https://hub.docker.com/_/microsoft-mssql-server

9.1 [SQL Server 2019 in Docker: You can accept the EULA by specifying the --accept-eula command line option](https://stackoverflow.com/questions/64319491/sql-server-2019-in-docker-you-can-accept-the-eula-by-specifying-the-accept-eu)

```
原因: 没有启动license , 可以试试docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=123456a@" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

9.2 : 连接到微软 SQL 服务器 您可以使用容器内的 sqlcmd 工具使用主机上的以下命令连接到 SQL 服务器 .这个账号密码是多少?

Sqlcmd: Error: Microsoft ODBC Driver 17 for SQL Server : Login failed for user '126810658'..

用 docker exec -it 9df70f9a0714 /bin/bash  进入container, 然后 看到有个安装的shell脚本, 里面说user 'mssql' 

忽然发现其实都有官方文档:https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash

还是官方文档好.下次可以来这里搜索https://docs.microsoft.com/zh-cn/

## tensorflow

[Docker  | TensorFlow](https://www.tensorflow.org/install/docker)

[How to Install TensorFlow on Ubuntu 20.04 | Atlantic.Net](https://www.atlantic.net/vps-hosting/how-to-install-tensorflow-on-ubuntu-20-04/)

报错有,但是太麻烦了, 懒得管他,用到再说把.

[Warning when trying run tensorflow with Docker on Windows - Stack Overflow](https://stackoverflow.com/questions/56817112/warning-when-trying-run-tensorflow-with-docker-on-windows)

我再 docker exec -it --user 123 container_name bash 这个可以,

显示:You are running this container as user with ID 123 and group 0,which should map to the ID and group for your user on the Docker host. Great!

映射自己的代码进container:

```
docker run -it --rm -v $PWD:/tmp -w /tmp tensorflow/tensorflow python ./script.py
```



## riscv

容器stop后,start了没有用, 可以用pause.



## 网络

docker ubuntu的网络 和host共享吗?

安装Docker时，它会自动创建三个网络，bridge（创建容器默认连接到此网络）、 none 、host

host：容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口。

Container：创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围。

None：该模式关闭了容器的网络功能。

Bridge：此模式会为每一个容器分配、设置IP等，并将容器连接到一个docker0虚拟网桥，通过docker0网桥以及Iptables nat表配置与宿主机通信。

[Docker：网络模式详解 - Gringer - 博客园 (cnblogs.com)](https://www.cnblogs.com/zuxing/articles/8780661.html)

#### dockerfile

dockerfile, 非常好用.可以看看经典仓库的dockerfile. 以后建环境的时候把过程记在dockerfile上, 因为dockfile 文件小不用下载很久. 
