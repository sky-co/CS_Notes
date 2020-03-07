### Docker
分清楚两个概念，容器和虚拟机
传统虚拟机如 VMware ， VisualBox 之类的需要模拟整台机器包括硬件，每台虚拟机都需要有自己的操作系统，虚拟机一旦被开启，预分配给它的资源将全部被占用。每一台虚拟机包括应用，必要的二进制和库，以及一个完整的用户操作系统。

而容器技术是和我们的宿主机共享硬件资源及操作系统，可以实现资源的动态分配。容器包含应用和其所有的依赖包，但是与其他容器共享内核。容器在宿主机操作系统中，在用户空间以分离的进程运行。

容器技术是实现操作系统虚拟化的一种途径，可以让您在资源受到隔离的进程中运行应用程序及其依赖关系。通过使用容器，我们可以轻松打包应用程序的代码、配置和依赖关系，将其变成容易使用的构建块，从而实现环境一致性、运营效率、开发人员生产力和版本控制等诸多目标。容器可以帮助保证应用程序快速、可靠、一致地部署，其间不受部署环境的影响。容器还赋予我们对资源更多的精细化控制能力，让我们的基础设施效率更高。

Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。

而 Linux 容器是 Linux 发展出了另一种虚拟化技术，简单来讲， Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离，相当于是在正常进程的外面套了一个保护层。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker ，就不用担心环境问题。

总体来说， Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

### 基本概念
Docker 中包括三个基本的概念：

Image(镜像)
Container(容器)
Repository(仓库)
镜像是 Docker 运行容器的前提，仓库是存放镜像的场所，可见镜像更是 Docker 的核心。

Image (镜像)
Docker 镜像可以看作是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。

镜像（Image）就是一堆只读层（read-only layer）的统一视角，也许这个定义有些难以理解，下面的这张图能够帮助读者理解镜像的定义。
统一文件系统 (union file system) 技术能够将不同的层整合成一个文件系统，为这些层提供了一个统一的视角，这样就隐藏了多层的存在，在用户的角度看来，只存在一个文件系统。

Container (容器)
容器 (container) 的定义和镜像 (image) 几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。
由于容器的定义并没有提及是否要运行容器，所以实际上，容器 = 镜像 + 读写层。


Repository (仓库)
Docker 仓库是集中存放镜像文件的场所。镜像构建完成后，可以很容易的在当前宿主上运行，但是， 如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry (仓库注册服务器)就是这样的服务。有时候会把仓库 (Repository) 和仓库注册服务器 (Registry) 混为一谈，并不严格区分。Docker 仓库的概念跟 Git 类似，注册服务器可以理解为 GitHub 这样的托管服务。实际上，一个 Docker Registry 中可以包含多个仓库 (Repository) ，每个仓库可以包含多个标签 (Tag)，每个标签对应着一个镜像。所以说，镜像仓库是 Docker 用来集中存放镜像文件的地方类似于我们之前常用的代码仓库。

通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本 。我们可以通过<仓库名>:<标签>的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签.。

仓库又可以分为两种形式：

public(公有仓库)
private(私有仓库)

Docker 使用 C/S 结构，即客户端/服务器体系结构。 Docker 客户端与 Docker 服务器进行交互，Docker服务端负责构建、运行和分发 Docker 镜像。 Docker 客户端和服务端可以运行在一台机器上，也可以通过 RESTful 、 stock 或网络接口与远程 Docker 服务端进行通信。

```
sudo usermod -a -G docker $USER
reboot
或者
usermod -aG docker jenkins
usermod -aG root jenkins
chmod 664 /var/run/docker.sock
But none of them work for me, I tried:

chmod 777 /var/run/docker.sock
```

**Refer**
1. https://docs.docker.com/install/linux/docker-ce/ubuntu/
2. https://docs.docker.com/install/linux/linux-postinstall/

### 运行例子
运行第一个容器
环境就绪，马上运行第一个容器，执行命令：
$ docker run -d -p 80:80 httpd
其过程可以简单的描述为：
从 Docker Hub 下载 httpd 镜像。镜像中已经安装好了 Apache HTTP Server。
启动 httpd 容器，并将容器的 80 端口映射到 host 的 80 端口。

### Docker的核心组件
Docker的核心组件包括
Docker Client
Docker daemon
Docker Image
Docker Registry
Docker Container

**Docker Client**
Docker Client ，也称 Docker 客户端。它其实就是 Docker 提供命令行界面 (CLI) 工具，是许多 Docker 用户与 Docker 进行交互的主要方式。客户端可以构建，运行和停止应用程序，还可以远程与Docker_Host进行交互。最常用的 Docker 客户端就是 docker 命令，我们可以通过 docker 命令很方便地在 host 上构建和运行 docker 容器。

**Docker daemon**
Docker daemon 是服务器组件，以 Linux 后台服务的方式运行，是 Docker 最核心的后台进程，我们也把它称为守护进程。它负责响应来自 Docker Client 的请求，然后将这些请求翻译成系统调用完成容器管理操作。该进程会在后台启动一个 API Server ，负责接收由 Docker Client 发送的请求，接收到的请求将通过Docker daemon 内部的一个路由分发调度，由具体的函数来执行请求。

**Docker Image**
Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。
Docker 镜像可以看作是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。我们可将 Docker 镜像看成只读模板，通过它可以创建 Docker 容器。

镜像有多种生成方法：
1. 从无到有开始创建镜像
2. 下载并使用别人创建好的现成的镜像
3. 在现有镜像上创建新的镜像
我们可以将镜像的内容和创建步骤描述在一个文本文件中，这个文件被称作 Dockerfile ，通过执行 docker build <docker-file> 命令可以构建出 Docker 镜像，在后续的教程中，我们会用一篇专门讨论这个问题。

一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。


**Docker Registry**
Docker registry 是存储 docker image 的仓库，它在 docker 生态环境中的位置如下图所示：
运行docker push、docker pull、docker search时，实际上是通过 docker daemon 与 docker registry 通信。

**Docker Container**
Docker 容器就是 Docker 镜像的运行实例，是真正运行项目程序、消耗系统资源、提供服务的地方。 Docker Container 提供了系统硬件环境，我们可以使用 Docker Images 这些制作好的系统盘，再加上我们所编写好的项目代码， run 一下就可以提供服务

### Docker组件是如何协作运行容器
```
$ docker pull library/hello-world
$ docker images
$ docker run hello-world
```
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
$ docker run -it ubuntu /bin/bash

Docker 容器的工作流程剖析的十分清楚了，我们大体可以知道 Docker 组件协作运行容器可以分为以下几个过程：
Docker 客户端执行 docker run 命令
Docker daemon 发现本地没有我们需要的镜像
daemon 从 Docker Hub 下载镜像
下载完成后，镜像被保存到本地
Docker daemon 启动容器

**可写的容器层**
当容器启动时，一个新的可写层被加载到镜像的顶部。
这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。
所有对容器的改动 - 无论添加、删除、还是修改文件都只会发生在容器层中。

只有容器层是可写的，容器层下面的所有镜像层都是只读的。
1. 添加文件
在容器中创建文件时，新文件被添加到容器层中。

2. 读取文件 
在容器中读取某个文件时，Docker 会从上往下依次在各镜像层中查找此文件。一旦找到，立即将其复制到容器层，然后打开并读入内存。

3. 修改文件 
在容器中修改已存在的文件时，Docker 会从上往下依次在各镜像层中查找此文件。一旦找到，立即将其复制到容器层，然后修改之。
   
4. 删除文件 
在容器中删除文件时，Docker 也是从上往下依次在镜像层中查找此文件。找到后，会在容器层中记录下此删除操作。

只有当需要修改时才复制一份数据，这种特性被称作 Copy-on-Write。可见，容器层保存的是镜像变化的部分，不会对镜像本身进行任何修改。
这样就解释了我们前面提出的问题：容器层记录对镜像的修改，所有镜像层都是只读的，不会被容器修改，所以镜像可以被多个容器共享。

### Dockerfile
Dockerfile 可以自定义镜像，通过 Docker 命令去运行镜像，从而达到启动容器的目的。

Dockerfile 分为四个部分：
基础镜像(父镜像)信息指令 FROM
维护者信息指令 MAINTAINER
镜像操作指令 RUN 、 EVN 、 ADD 和 WORKDIR 等
容器启动指令 CMD 、 ENTRYPOINT 和 USER 等

**Dockerfile常用的指令**
FROM
FROM 是用于指定基础的 images ，一般格式为 FROM <image> or FORM <image>:<tag> ，所有的 Dockerfile 都用该以 FROM 开头，FROM 命令指明 Dockerfile 所创建的镜像文件以什么镜像为基础，FROM 以后的所有指令都会在 FROM 的基础上进行创建镜像。

MAINTAINER
MAINTAINER 是用于指定镜像创建者和联系方式，一般格式为 MAINTAINER <name> 。

COPY
COPY 是用于复制本地主机的 <src> (为 Dockerfile 所在目录的相对路径)到容器中的 <dest>。

WORKDIR
WORKDIR 用于配合 RUN，CMD，ENTRYPOINT 命令设置当前工作路径。可以设置多次，如果是相对路径，则相对前一个 WORKDIR 命令。默认路径为/。一般格式为 WORKDIR /path/to/work/dir 。

RUN
RUN 用于容器内部执行命令。每个 RUN 命令相当于在原有的镜像基础上添加了一个改动层，原有的镜像不会有变化。一般格式为 RUN <command> 。

EXPOSE
EXPOSE 命令用来指定对外开放的端口。一般格式为 EXPOSE <port> [<port>...]

ENTRYPOINT
ENTRYPOINT 可以让你的容器表现得像一个可执行程序一样。一个 Dockerfile 中只能有一个 ENTRYPOINT，如果有多个，则最后一个生效。
ENTRYPOINT 命令也有两种格式：
ENTRYPOINT ["executable", "param1", "param2"] ：推荐使用的 exec形式
ENTRYPOINT command param1 param2 ：shell 形式

CMD
CMD 命令用于启动容器时默认执行的命令，CMD 命令可以包含可执行文件，也可以不包含可执行文件。不包含可执行文件的情况下就要用 ENTRYPOINT 指定一个，然后 CMD 命令的参数就会作为ENTRYPOINT的参数。
CMD 命令有三种格式：
CMD ["executable","param1","param2"]：推荐使用的 exec 形式。
CMD ["param1","param2"]：无可执行程序形式
CMD command param1 param2：shell 形式。
一个 Dockerfile 中只能有一个CMD，如果有多个，则最后一个生效。而 CMD 的 shell 形式默认调用 /bin/sh -c 执行命令。
CMD 命令会被 Docker 命令行传入的参数覆盖：docker run busybox /bin/echo Hello Docker 会把 CMD 里的命令覆盖。

### 构建镜像
1. docker commit
docker commit 命令是创建新镜像最直观的方法，其过程包含三个步骤：
运行容器
修改容器
将容器保存为新的镜像

2. Dockerfile
Dockerfile 是一个文本文件，记录了镜像构建的所有步骤
从 base 镜像运行一个容器。
执行一条指令，对容器做修改。
执行类似 docker commit 的操作，生成一个新的镜像层。
Docker 再基于刚刚提交的镜像运行一个新容器。
重复 2-4 步，直到 Dockerfile 中的所有指令执行完毕。
（如果 Dockerfile 由于某种原因执行到某个指令失败了，我们也将能够得到前一个指令成功执行构建出的镜像，这对调试 Dockerfile 非常有帮助）
**注意**：Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大

**调试方法：**
docker run -it 镜像id // 启动镜像的一个容器

两种进入容器的方法，不过前提是这个容器是正在运行中alive的
在使用 -d 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入

1. docker attach
通过 docker attach 可以 attach 到容器启动命令的终端。
注：可通过 Ctrl+p 然后 Ctrl+q 组合键退出 attach 终端。

2. docker exec -it container_id /bin/bash
可以像在普通 Linux 中一样执行命令。ps -elf 显示了容器启动进程
docker exec -it <container> bash|sh 是执行 exec 最常用的方式。

3. attach VS exec
attach 与 exec 主要区别如下:
attach 直接进入容器 启动命令 的终端，不会启动新的进程。
exec 则是在容器中打开新的终端，并且可以启动新的进程。
如果想直接在终端中查看启动命令的输出，用 attach；其他情况使用 exec。
当然，如果只是为了查看启动命令的输出，可以使用 docker logs 命令.
（推荐大家使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。）


**RUN vs CMD vs ENTRYPOINT**
1. RUN:执行命令并创建新的镜像层。RUN常用于安装软件包
2. CMD：设置容器启动后默认执行的命令及其参数，但CMD能被docker run 后面跟的命令行参数替换
3. ENTRYPOINT：配置容器启动时运行的命令

**最佳实践**
1. 使用 RUN 指令安装应用和软件包，构建镜像。
2. 如果 Docker 镜像的用途是运行应用程序或服务，比如运行一个 MySQL，应该优先使用 Exec 格式的 ENTRYPOINT 指令。CMD 可为 ENTRYPOINT 提供额外的默认参数，同时可利用 docker run 命令行替换默认参数。
3. 如果想为容器设置默认的启动命令，可使用 CMD 指令。用户可在 docker run 命令行中替换此默认命令。

### 容器的生存周期
容器的生命周期依赖于启动时执行的命令，只要该命令不结束，容器也就不会退出。
理解了这个原理，我们就可以通过执行一个长期运行的命令来保持容器的运行状态。例如执行下面的命令
$ docker run 镜像名称/id /bin/bash -c "while true ; do sleep 1; done"
(while 语句让 bash 不会退出)

不过这种方法有个缺点：它占用了一个终端。我们可以加上参数 -d 以后台方式启动容器

**通过 docker ps 查看容器**
现在我们有了两个正在运行的容器。这里注意一下容器的 CONTAINER ID和 NAMES 这两个字段。
1. CONTAINER ID 是容器的 “短ID”，前面启动容器时返回的是 “长ID”。短ID是长ID的前12个字符。
2. NAMES 字段显示容器的名字，在启动容器时可以通过 --name 参数显式地为容器命名，如果不指定，docker 会自动为容器分配名字。

### 容器的底层实现技术。
cgroup 和 namespace 是最重要的两种技术。
1. cgroup 实现资源限额
2. namespace 实现资源隔离

### Docker 存储
容器由最上面一个可写的容器层，以及若干只读的镜像层组成，容器的数据就存放在这些层中。这样的分层结构最大的特性是 Copy-on-Write：
新数据会直接存放在最上面的容器层。
修改现有数据会先从镜像层将数据复制到容器层，修改后的数据直接保存在容器层中，镜像层保持不变。
如果多个层中有命名相同的文件，用户只能看到最上面那层中的文件。

**Data Volume**
Data Volume 本质上是 Docker Host 文件系统中的目录或文件，能够直接被 mount 到容器的文件系统中。Data Volume 有以下特点：
Data Volume 是目录或文件，而非没有格式化的磁盘（块设备）。
容器可以读写 volume 中的数据。
volume 数据可以被永久的保存，即使使用它的容器已经销毁。
(volume 实际上是 docker host 文件系统的一部分，所以 volume 的容量取决于文件系统当前未使用的空间，目前还没有方法设置 volume 的容量)

**1. bind mount**
bind mount 是将 host 上已存在的目录或文件 mount 到容器。

例如 docker host 上有目录 $HOME/htdocs：
// index.html
<html><body><h1>This is a file in host file system!</h1></body></html>

通过 -v 将其 mount 到 httpd 容器：

```docker
docker run -d -p 80:80 -v ~/htdocs:/usr/local/apache2/htdocs httpd
```

-v 的格式为 <host path>:<container path>。/usr/local/apache2/htdocs 就是 apache server 存放静态文件的地方。由于 /usr/local/apache2/htdocs 已经存在，原有数据会被隐藏起来，取而代之的是 host $HOME/htdocs/ 中的数据，这与 linux mount 命令的行为是一致的。
bind mount 可以让 host 与容器共享数据。这在管理上是非常方便的。
将容器销毁,bind mount 也还在。bind mount 是 host 文件系统中的数据，只是借给容器用用

bind mount 时还可以指定数据的读写权限，默认是可读可写，可指定为只读：

```docker
docker run -d -p 80:80 -v ~/htdocs:/usr/local/apache2/htdocs：ro httpd
```

bind mount 单个文件的场景是：只需要向容器添加文件，不希望覆盖整个目录
使用单一文件有一点要注意：host 中的源文件必须要存在，不然会当作一个新目录 bind mount 给容器。

```docker
docker run -d -p 80:80 -v ~/htdocs/index.html:/usr/local/apache2/htdocs/index.html httpd
```

缺点：
bind mount 的使用直观高效，易于理解，但它也有不足的地方：bind mount 需要指定 host 文件系统的特定路径，这就限制了容器的可移植性，当需要将容器迁移到其他 host，而该 host 没有要 mount 的数据或者数据不在相同的路径时，操作会失败。

**2. docker managed volume**
docker managed volume 的创建过程：
(1)容器启动时，简单的告诉 docker "我需要一个 volume 存放数据，帮我 mount 到目录 /abc"。
(2)docker 在 /var/lib/docker/volumes 中生成一个随机目录作为 mount 源。
(3)如果 /abc 已经存在，则将数据复制到 mount 源，将 volume mount 到 /abc
(4)除了通过 docker inspect 查看 volume，我们也可以用 docker volume 命令：`docker volume ls`
目前，docker volume 只能查看 docker managed volume，还看不到 bind mount；同时也无法知道 volume 对应的容器，这些信息还得靠docker inspect。
`docker volume inspect volume_id`
(使用 docker inspect 来查看 Docker 的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息)

**数据共享**
1. volume container
   数据保存在host中，不必为每一个容器指定 host path，所有 path 都在 volume container 中定义好了，容器只需与 volume container 关联，实现了容器与 host 的解耦。
2. data-packed volume container
   将数据打包到镜像中，然后通过 docker managed volume 共享

### 常用命令
1. docker ps -a
2. docker build -t 镜像名称 . // 从 Dockerfile 构建指定名称的镜像
3. docker run -it 镜像id  /bin/bash // 运行并镜像，-t:在新容器内指定一个伪终端或终端； -i:允许你对容器内的标准输入 (STDIN) 进行交互。
   （可以通过运行 exit 命令或者使用 CTRL+D 来退出容器）
4. docker images  //显示镜像列表
5. docker history image id // 会显示镜像的构建历史，也就是 Dockerfile 的执行过程
6. docker commit 容器名称 镜像名称  // 从容器创建新镜像
7. docker tag 镜像名称  // 给镜像打 tag
8. docker pull 镜像名称 // 从 registry 下载镜像
9.  docker push 镜像名称 // 将 镜像 上传到 registry
10. docker rmi 镜像id // 删除 Docker host 中的镜像
11. docker search  镜像id //搜索 Docker Hub 中的镜像
12. docker logs container_id // 显示容器启动进程的控制台输出，用-f持续打印
13. stop/start/restart 容器
docker start 会保留容器的第一次启动时的所有参数。
docker restart 可以重启容器，其作用就是依次执行 docker stop 和docker start。
容器在 docker host 中实际上是一个进程，docker stop 命令本质上是向该进程发送一个 SIGTERM 信号。如果想快速停止容器，可使用 docker kill 命令，其作用是向容器进程发送 SIGKILL 信号
容器可能会因某种错误而停止运行。对于服务类容器，我们通常希望在这种情况下容器能够自动重启。启动容器时设置 --restart 就可以达到这个效果。
--restart=always 意味着无论容器因何种原因退出（包括正常退出），就立即重启。该参数的形式还可以是 --restart=on-failure:3，意思是如果启动进程退出代码非0，则重启容器，最多重启3次。
14. pause/unpause 容器
有时我们只是希望暂时让容器暂停工作一段时间，比如要对容器的文件系统打个快照，或者 dcoker host 需要使用 CPU，这时可以执行 docker pause。
处于暂停状态的容器不会占用 CPU 资源，直到通过 docker unpause 恢复运行
15. 删除容器
docker rm 容器 // 一次可以指定多个容器，如果希望批量删除所有已经退出的容器，可以执行如下命令：
docker rm -v $(docker ps -aq -f status=exited)
(顺便说一句：docker rm 是删除容器，而 docker rmi 是删除镜像)
16. docker volume rm volume_id
    或者删除容器时候带-v，docker会将容器用到的volume一并删除，前提是没有其他容器mount该volume

**指定容器的三种方法：**
1. 短ID。
2. 长ID。
3. 容器名称。 可通过 --name 为容器命名。重命名容器可执行docker rename

**容器按用途可分为两类：**
1. 服务类的容器。
服务类容器以 daemon 的形式运行，对外提供服务。比如 web server，数据库等。通过 -d 以后台方式启动这类容器是非常合适的。如果要排查问题，可以通过 exec -it 进入容器。
2. 工具类的容器
工具类容器通常给能我们提供一个临时的工作环境，通常以 run -it 方式运行

### Docker网络
安装brctl命令
```
sudo apt install bridge-utils
```

### Docker Compose
Compose 简介
Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。
如果你还不了解 YML 文件配置，可以先阅读 [YAML](https://www.runoob.com/w3cnote/yaml-intro.html) 入门教程。
Compose 使用的三个步骤：
使用 Dockerfile 定义应用程序的环境。
使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
最后，执行 docker-compose up 命令来启动并运行整个应用程序。

### 镜像加速
请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）：
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
EOF

之后重新启动服务：
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker

**Docker 国内镜像**
阿里云的加速器：https://help.aliyun.com/document_detail/60750.html
网易加速器：http://hub-mirror.c.163.com
官方中国加速器：https://registry.docker-cn.com
ustc 的镜像：https://docker.mirrors.ustc.edu.cn
daocloud：https://www.daocloud.io/mirror#accelerator-doc（注册后使用）

### 资源
1. https://blog.csdn.net/deng624796905/article/details/86493330