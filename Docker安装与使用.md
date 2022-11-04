## Docker安装与使用教程
##### 小提示1：
##### 本教程所有执行的命令都是基于`root`账号下执行的，如果你的linux系统不是用`root`账号登录的，执行每条命令的时候请手动在命令的前边加上`sudo`；
##### 比如:
    sudo 命令
    sudo apt update -y
    sudo yum update -y
##### 小提示2：
##### 有些服务器在执行带有`sudo`命令的时候可能会提示`-bash: sudo: command not found`，这是因为有些服务商的服务器在安装系统的时候没有安装`sudo`指令模块，可能是由于`sudo`模块的权限太高被商家精简掉了，也可能是系统问题或者是机器硬件问题导致`sudo`指令模块没有安装上，可以使用以下两种方法解决
##### 1)
##### Debian / Ubuntu
    apt install -y sudo
##### Cent OS
    yum install -y sudo
##### 2)
    将命令中所有的`sudo`字段都删除掉，当然这种方法有一个必要条件就是你的linux必须是以`root`账号登录的，因为在root账户下运行的命令加不加sudo；效果其实是一样的;
##### 小提示3：
##### 本教程中关于`apt`命令,在此向比我还‘白’的那些小白解释一下，你会发现有的命令用的是`apt+指令`， 有的命令中用的却是`apt-get+指令`，不用纠结这个，其实在`Debian7.6`到最新的系统和`Ubuntu20.10`到最新的系统中这两个命令是一样的，都可以用，有的服务商的服务器(或系统)可能存在问题或者是服务商有意做了限制，导致只能使用`apt`或`apt-get`其中的一个运行指令，如果在运行命令的时候碰到`apt`或`apt-get`相关的报错，导致命令没有成功执行，所以就麻烦各位小白自己手动更改一下教程中的`apt`或`apt-get`，再次运行应该就可以了
##### 小提示4：
##### 如果碰到教程中没有提到的错误，请不要慌，要学会和习惯使用`百度`和`Google`帮你解决问题，将错误的关键字或者整条错误消息复制黏贴到`百度`或者`Google`的搜索栏里，也可以将你的问题、报错信息截图什么的(尽量能清楚详细的表述清楚你的问题)的东西在`百度提问`里或`Google`提问里或者某一个国内的国外的论坛里发一个贴子，肯定会有人帮你解答问题的，将搜索到的结果挨个阅读，这里最关键的就是要有执行力，不怕麻烦跟着搜索到的解决办法一步一步的在自己的`linux`服务器里执行操作，不要怕把服务器搞坏，大不了在你服务器的控制台里再重新安装一个操作系统，再者说你是花了钱的怕什么；
##### 小提示5：
##### 在开始安装新版本之前最好执行一下以下的命令，它会判断出当前系统是否存在已安装的旧版本的`Docker`并且卸载旧版本的`Docker`,如果报告没有安装这些软件包，那也没关系
##### Debian / Ubuntu
    apt-get remove docker docker-engine docker.io containerd runc
##### Cent SO
    yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
>卸载`Docker`时，不会自动删除其中存储的映像、容器、卷和网络。如果您想从全新安装开始，并希望清理任何现有数据，请参阅 卸载`Docker`引擎部分
### 开始安装
### 1. 更新系统核心文件
#### Debian / Ubuntu
    apt update -y && apt upgrade && apt autoremove
#### Cent OS
    yum update -y && yum upgrade && yum autoremove
#### 安装yum-utils包(仅`Cent OS`系统需要)
##### 官网的(首选)
    yum install -y yum-utils
##### 以下是网上的
    sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
### 2. 重启系统
    reboot
### 3. 安装存储库
#### 官网的(首选)
##### Debian / Ubuntu
    apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
##### Cent OS
    yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#### 以下是网上的
##### Deban
    apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common
##### Ubuntu
    apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
##### Cent OS(用国内的镜像源地址比官方的快一点，镜像源地址失效了可以自己找新的替换)
    yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
### 4.获取Docker官方的GPG密钥(仅`Debian`和`Ubuntu`需要，`Cent OS`请直接看第11步)
#### 官网的(首选)
#### 1)
    mkdir -p /etc/apt/keyrings
#### 2)
##### Debian
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
##### Ubuntu
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
#### 以下是网上的
##### Debian
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
##### Ubuntu
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
### 5. 查看`GPG密钥`(一般这个密钥是固定的，官方文档里也可以找的到，无论哪个设备安装都是一样的密钥，之所以做这一步是防止官方哪天密钥变了，文档还没更新)
    apt-key list
### 6. 验证此`GPG密钥`的指纹(这条命令后边8位字母和数字是你的`GPG密钥`的后8位；一般这条命令直接复制执行就行，但是为了防止官方密钥改变最好将你上一步查出来的`GPG密钥`的后8位和这条命令上的对比一下，如果不一样就替换成你的)
    apt-key fingerprint 0EBFCD88
### 7. 设置存储库
#### 官网的(首选)
##### Debian
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
##### Ubuntu
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
#### 以下是网上的
##### Debian
    add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/debian \
    $(lsb_release -cs) \
    stable"
##### Ubuntu
    add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
### 8. 查看OS采用的核心号(可选)
    lsb_release -cs
### 9. 查看存储库加入的位置和内容(可选)
    cat /etc/apt/sources.list|grep docker
### 10. 授予Docker 公钥文件的读取权限(可选，再次更新系统碰到提示:`运行时收到 GPG 错误apt-get update`时执行) 
    chmod a+r /etc/apt/keyrings/docker.gpg
### 11. 再次更新系统
##### Debian / Ubuntu
    apt update -y && apt upgrade
##### Cent OS
    yum update -y && yum upgrade
### 12. 验证Docker-ce是否已经安装成功
    apt show docker-ce
### 13. 安装`Docker`引擎(或者可以看第14步和第15步指定版本安装)
#### 官网的(首选)
##### Debian / Ubuntu
    apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
##### Cent OS(如果提示你接受GPG密钥验证，请选择是)
    yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
#### 以下是网上的
##### Debian / Ubuntu
    apt-get install docker-ce docker-ce-cli containerd.io
### 14. 列出存储库中的可用版本
##### Debian / Ubuntu
    apt-cache madison docker-ce | awk '{ print $3 }'
##### Cent OS
    yum list docker-ce --showduplicates | sort -r
### 15. 指定Docker引擎的版本安装(可选)
##### Debian / Ubuntu
#### 1)先指定版本(对于新安装或者从低版本升级到高版本可以使用这种方法)
    VERSION_STRING=5:18.09.0~3-0~debian-stretch
#### 2)再安装指定版本(如果降级安装使用此方法会报错)
    apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-compose-plugin
#### 降级指定版本的时候直将命令中的`$VERSION_STRING`替换为指定版本
    apt-get install docker-ce=5:20.10.20~3-0~ubuntu-jammy docker-ce-cli=5:20.10.20~3-0~ubuntu-jammy containerd.io docker-compose-plugin
##### Cent OS
    yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io docker-compose-plugin
#### 例
##### 依据官方的说法将命令中的`<VERSION_STRING>`替换为上一步查出来的你想指定的版本的包名称+版本字符串（第 2 列），从第一个冒号 ( :) 开始，一直到第一个连字符，用连字符 ( -) 分隔。例如，docker-ce-18.09.1。

    yum install docker-ce-docker-ce-18.09.1 docker-ce-cli-docker-ce-18.09.1 containerd.io docker-compose-plugin
### 16. 启动Docker
    systemctl start docker
### 17. 停止Docker服务
    systemctl stop docker
##### 在一些服务器上使用systemctl启动或停止`Docker`可能会报错，比如win10或win11的linux子系统的环境下是无法启动的，会出现如下的错误

    System has not been booted with systemd as init system (PID 1). Can't operate.
    Failed to connect to bus: 主机关闭
##### 解决办法就是用下面这个命令启动

    service docker start
##### 停止Docker服务

    service docker stop
### 18. 卸载Docker
##### Debian / Ubuntu
    apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
##### Cent OS
    yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
### 19. 主机上的映像、容器、卷或自定义配置文件不会自动删除。要删除所有映像、容器和卷
>rm -rf /var/lib/docker

>rm -rf /var/lib/containerd

您必须手动删除任何已编辑的配置文件
### 20. 查看`Docker`安装目录的命令
    whereis docker
## Docker镜像加速
### 1. 国内拉取`DockerHub`镜像加速器
#### 科大镜像：
    https://docker.mirrors.ustc.edu.cn/
#### 网易：
    https://hub-mirror.c.163.com/
#### 七牛云加速器：
    https://reg-mirror.qiniu.com
### 2. 配置`Docker`加速器
#### 1）Ubuntu14.04、Debian7Wheezy
##### 对于使用 upstart 的系统而言，编辑 /etc/default/docker 文件，在其中的 DOCKER_OPTS 中配置加速器地址：
    DOCKER_OPTS="--registry-mirror=https://registry.docker-cn.com"
#### 2) Ubuntu16.04+、Debian8+、CentOS7
##### 对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）：
    {"registry-mirrors":["https://reg-mirror.qiniu.com/"]}
### 3. 重启Docker服务
#### 1) Ubuntu14.04、Debian7Wheezy
    service docker restart
#### 2) Ubuntu16.04+、Debian8+、CentOS7
    systemctl daemon-reload && systemctl restart docker
### 4. 检查加速器是否生效
    docker info
## `可能会用到的Linux命令`
### 1. 查看容器的核心版本
    cat /etc/os-release
### 2. 列出nginx设置目录
    ls -R -l /etc/nginx
### 3. 查看nginx全局设置文件
    cat /etc/nginx/nginx.conf
### 4. 查看默认Web虚拟主机设置文件
    cat /etc/nginx/conf.d/default.conf
### 5. 查看虚拟目录下面的内容
    ls -R -l /usr/share/nginx/html
### 6. 服务动作确认
    curl http://127.0.0.1:8088
### 7. 建立本地的Web目录
    mkdir myweb
### 9. 建立文件
    nano index.html
## 可能会用到的`Docker`命令
### 容器的生命周期
### `Docker run`命令:创建一个新的容器并运行一个命令
#### 1. 指定标准输入输出内容类型，可选 `STDIN/STDOUT/STDERR` 三项
    -a stdin
#### 2. 为容器指定一个名称
    --name="nginx-lb"
#### 3. 以交互模式运行容器，通常与 `-t` 同时使用
    -i
#### 4. 为容器重新分配一个伪输入终端，通常与 `-i` 同时使用
    -t
#### 5. 后台运行容器，并返回`容器ID`
    -d
#### 6. 指定容器使用的`DNS`服务器，默认和宿主一致
    --dns 8.8.8.8
#### 7. 指定容器`DNS`搜索域名，默认和宿主一致
    --dns-search example.com
#### 8. 指定容器的`hostname`
    -h "mars"
#### 9. 将主机中项目的目录 `www` 挂载到容器的 `/www`
    -v ~/nginx/www:/www
#### 10. 将容器内部使用的网络端口随机映射到我们使用的主机上
    -P
#### 11. 将容器内部使用的网络端口映射到主机上指定的端口
    -p
#### 12. 设置容器使用内存的最大值
    -m
#### 13. 指定容器的网络连接类型，支持 `bridge/host/none/container`: 四种类型
    --net="bridge"
#### 14. 设置环境变量
    -e username="ritchie"
#### 15. 从指定文件读入环境变量
    --env-file=[]
#### 16. 绑定容器到指定CPU运行
    --cpuset="0-2" or --cpuset="0,1,2"
#### 17. 添加链接到另一个容器
    --link=[]
#### 18. 开放一个端口或一组端口
    --expose=[]
### `Docker kill`命令:杀掉一个运行中的容器
#### 1. 向容器发送一个信号
    -s
### `Docker rm`命令:删除一个或多个容器
#### 1. 通过 `SIGKILL` 信号强制删除一个运行中的容器
    -f
#### 2. 移除容器间的网络连接，而非容器本身
    -l
#### 3. 删除与容器关联的卷
    -v
#### 4. 让 `docker logs` 像使用 `tail -f` 一样来输出容器内部的标准输出
    -f
#### 5. 查询最后一次创建的容器
    -l
### `Docker pause/unpause`命令:暂停或恢复容器中所有的进程
#### 1. 暂停容器
    pause
#### 2. 恢复容器
    unpause
### `Docker create`命令:创建一个新的容器但不启动它
#### 1. 命名容器
    --name
### `Docker exec`命令:在运行的容器中执行命令
#### 1. 分离模式，在后台运行
    -d
#### 2. 即使没有附加也保持`STDIN`打开
    -i
#### 3. 分配一个伪终端
    -t
### 容器操作
### `Docker ps`命令:列出容器
#### 1. 显示所有的容器，包括未运行的
    -a
#### 2. 根据条件过滤显示的内容
    -f
#### 3. 指定返回值的模板文件
    --format
#### 4. 显示最近创建的容器
    -l
#### 5. 最近创建的n个目录
    -n 目录
#### 6. 不截断输出
    --no-trunc
#### 7. 静默模式，只显示容器编号
    -q
#### 8. 显示总的文件大小
    -s
### `Docker inspect` 命令:获取容器/镜像的元数据
#### 1. 指定返回值的模板文件
    -f
#### 2. 显示总的文件大小
    -s
#### 3. 为指定类型返回`JSON`
    --type
### `Docker top`命令:查看容器中运行的进程信息，支持 `ps` 命令参数
### `Docker attach`命令:连接到正在运行中的容器
#### 1. 确保`CTRL-D`或`CTRL-C`不会关闭容器
    --sig-proxy=false
### `Docker events`命令从服务器获取实时事件
#### 1. 根据条件过滤事件
    -f
#### 2. 从指定的时间戳后显示所有事件
    --since
#### 3. 流水时间显示到指定的时间为止
    --until
### `Docker logs` 命令:获取容器的日志
#### 1. 跟踪日志输出
    -f
#### 2. 显示某个开始时间的所有日志
    --since
#### 3. 显示时间戳
    -t
#### 4. 仅列出最新N条容器日志
    --tail
### `Docker wait` 命令:阻塞运行直到容器停止，然后打印出它的退出代码
### `Docker export` 命令:将文件系统作为一个`tar`归档文件导出到`STDOUT`
#### 1. 将输入内容写到文件
    -o
### `Docker port` 命令:用于列出指定的容器的端口映射，或者查找将 `PRIVATE_PORT NAT` 到面向公众的端口
### `Docker stats` 命令:显示容器资源的使用情况，包括：`CPU`、内存、网络 `I/O` 等
#### 1. 显示所有的容器，包括未运行的
    --all 或者 -a
#### 2. 指定返回值的模板文件
    --format
#### 3. 展示当前状态就直接退出了，不再实时更新
    --no-stream
#### 4. 不截断输出
    --no-trunc
### 容器`rootfs`命令
### `Docker commit` 命令:从容器创建一个新的镜像
#### 1. 提交的镜像作者
    -a
#### 2. 使用`Dockerfile`指令来创建镜像
    -c
#### 3. 提交时的说明文字
    -m
#### 4. 在`commit`时，将容器暂停
    -p
### `Docker cp` 命令:用于容器与主机之间的数据拷贝
#### 1. 保持源目标中的链接
    -L
### `Docker diff` 命令:检查容器里文件结构的更改
### 镜像仓库命令
### `Docker login/logout` 命令:登陆或登出`Docker`镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 `Docker Hub`
#### 1. 登陆的用户名
    -u
#### 2. 登陆的密码
    -p
### `Docker pull` 命令:从镜像仓库中拉取或者更新指定镜像
#### 1. 拉取所有 `tagged` 镜像
    -a
#### 2. 忽略镜像的校验,默认开启
    --disable-content-trust
### `Docker push` 命令:将本地的镜像上传到镜像仓库,要先登陆到镜像仓库
#### 1. 忽略镜像的校验,默认开启
    --disable-content-trust
### `Docker search` 命令:从`Docker Hub`查找镜像
#### 1. 只列出 `automated build`类型的镜像
    --automated
#### 2. 显示完整的镜像描述
    --no-trunc
#### 3. 列出收藏数不小于指定值的镜像
    -f <过滤条件>
### 本地镜像管理
### `Docker images` 命令:列出本地镜像
#### 1. 列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）
    -a
#### 2. 显示镜像的摘要信息
    --digests
#### 3. 显示满足条件的镜像
    -f
#### 4. 指定返回值的模板文件
    --format
#### 5. 显示完整的镜像信息
    --no-trunc
#### 6. 只显示镜像`ID`
    -q
### `Docker rmi` 命令:删除本地一个或多个镜像
#### 1. 强制删除
    -f
#### 2. 不移除该镜像的过程镜像，默认移除
    --no-prune
### `Docker tag` 命令:标记本地镜像，将其归入某一仓库
### `Docker build` 命令:命令用于使用 `Dockerfile` 创建镜像
#### 1. 设置镜像创建时的变量
    --build-arg=[]
#### 2. 设置 `cpu` 使用权重
    --cpu-shares
#### 3. 限制 `CPU CFS`周期
    --cpu-period
#### 4. 限制 `CPU CFS`配额
    --cpu-quota
#### 5. 指定使用的`CPU id`
    --cpuset-cpus
#### 6. 指定使用的内存 `id`
    --cpuset-mems
#### 7. 忽略校验，默认开启
    --disable-content-trust
#### 8. 指定要使用的`Dockerfile`路径
    -f
#### 9. 设置镜像过程中删除中间容器
    --force-rm
#### 10. 使用容器隔离技术
    --isolation
#### 11. 设置镜像使用的元数据
    --label=[]
#### 12. 设置内存最大值
    -m
#### 13. 设置`Swap`的最大值为内存+swap，`-1`表示不限`swap`
    --memory-swap
#### 14. 创建镜像的过程不使用缓存
    --no-cache
#### 15. 尝试去更新镜像的新版本
    --pull
#### 16. 安静模式，成功后只输出镜像 ID
    --quiet, -q
#### 17. 设置镜像成功后删除中间容器
    --rm
#### 18. 设置`/dev/shm`的大小，默认值是`64M`
    --shm-size
#### 19. `Ulimit`配置
    --ulimit
#### 20. 将 `Dockerfile` 中所有的操作压缩为一层
    --squash
#### 21. 镜像的名字及标签，通常 `name:tag` 或者 `name` 格式；可以在一次构建中为一个镜像设置多个标签
    --tag, -t
#### 22. 默认 `default`。在构建期间设置RUN指令的网络模式
    --network
### `Docker history` 命令:查看指定镜像的创建历史
#### 1. 以可读的格式打印镜像大小和日期，默认为`true`
    -H
#### 2. 显示完整的提交记录
    --no-trunc
#### 3. 仅列出提交记录`ID`
    -q
### `Docker save` 命令:将指定镜像保存成 `tar` 归档文件
#### 1. 输出到的文件
    -o
### `Docker load` 命令:导入使用 `docker save` 命令导出的镜像
#### 1. 指定导入的文件，代替 `STDIN`
    --input , -i
#### 2. 精简输出信息
    --quiet , -q
### `Docker import` 命令:从归档文件中创建镜像
#### 1. 应用docker 指令创建镜像
    -c
#### 2. 提交时的说明文字
    -m
### `info`|`version`
### `Docker info` 命令:显示 `Docker` 系统信息，包括镜像和容器数
### `Docker version` 命令:显示 Docker 版本信息
#### 1. 指定返回值的模板文件
    -f
## 开始使用
### 常用的`Docker`容器管理命令
### 1. 查看所有容器
    docker ps -a
### 2.查看正在运行的容器
    docker ps
### 3. 查询最后一次创建的容器
    docker ps -l
### 4. 从`DockerHub`拉取镜像
#### `1) 访问`[DockerHub官网](https://hub.docker.com)；
#### `2) 在搜索框里搜索你想要拉取的镜像名称;`
#### `3) 点击搜索出来的结果中你想拉取的镜像名称进入镜像的详情展示页`
#### `4) 点击详情页面中的` Tags `标签页`
#### `5) 在` Tags `页面中找到你想要拉取的镜像的版本号`
#### `6) 版本号后边对应的有一个带有复制按钮的区域，那里就是镜像的拉取命令`
#### `7) 复制镜像的拉取命令到你安装了` Docker `环境的服务器上的终端命令行里运行`
#### 例：
    docker pull tomcat:7.0.57
### 5. 查看所有下载下来的镜像(二选一)
#### 1)
    docker image ls
#### 2)
    docker images
### 6.使用镜像启动一个容器(创建容器)
#### 1) 对于像系统之类的镜像创建容器的时候要用这条命令(前台运行)
    docker run -itd --name 容器名称 镜像名称 参数
#### 例
    docker run -itd --name ubuntu ubuntu /bin/bash
>关于`/bin/bash`这个参数的意思是创建一个bash终端，当然你也可以指定别的参数
#### 2) 对于像`Tomcat`之类的web服务器镜像创建容器的时候要用这条命令
    docker run --name 容器名称 -p 服务器端口:容器服务端口 -d 镜像名称
#### 例
    docker run --name tomcat -p 8080:8080 -d tomcat:7.0.57
#### 3) 对于像MySQL之类的数据库服务器创建容器的时候要用这条命令
    docker run -itd --name 容器名称 -p 服务器端口:容器服务端口 -e MYSQL_ROOT_PASSWORD=root密码 镜像名称
#### 例
    docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.6
#### 将容器中的网站文件映射到服务器本地(`容器重启后修改的配置不会丢失`)
    docker run --name 容器名称 -p 服务器端口:容器服务端口 -d -v 服务器(本地)的web文件目录:容器web文件目录:ro 镜像名称
#### 例
    docker run --name tomcat -p 8080:8080 -d -v /home/lcadmin/myweb:/usr/share/nginx/html:ro tomcat:7.0.57
### 9. 查看容器内目录
    docker exec 容器名称 系统命令
#### 例
    docker exec tomcat ls
### 10. 容器启动和停止
#### 1)启动
    docker start 容器名称或容器ID
#### 2)停止
    docker stop 容器名称或容器ID
#### 3)重启
    docker restart 容器名称或容器ID
### 11. 连接容器(对于系统类容器需使用此命令)
    docker exec -it 容器名称或容器ID /bin/sh
### 12. 退出容器连接
    exit
### 13. 删除容器(删除之前一定要先停止容器)
#### 1) 删除单个容器
    docker rm 容器名称或容器ID
#### 2) 删除所有容器
    docker prune
### 14. 删除镜像
#### 1)
    docker rmi 镜像名称或镜像ID
#### 2)
    docker image rm 镜像名称或镜像ID
### 15. 导出容器
    docker export 容器名称或容器ID > 导出文件的名称和后缀
#### 例
    docker export 1e560fca3906 > ubuntu.tar
### 16. 导入容器
#### 1)使用 docker import 从容器快照文件中再导入为镜像
    cat 服务器本地目录/容器快照 | docker import - test/ubuntu:v1
#### 例(将快照文件 ubuntu.tar 导入到镜像 test/ubuntu:v1:)
    cat docker/ubuntu.tar | docker import - test/ubuntu:v1
#### 2)通过指定 URL 或者某个目录来导入
    docker import http://example.com/exampleimage.tgz example/imagerepo
### 17. 查看指定容器的端口绑定情况
    docker port 容器名称或容器ID 后面还可以指定要查询的端口号
### 18. 查看web容器的日志
    docker logs -f 容器名称或容器ID
### 19. 查看容器内部运行的进程
    docker top 容器名称或容器ID
### 20. 查看 Docker 的容器的配置和状态信息
    docker inspect 容器名称或容器ID
### 21. 用命令寻找一个镜像
    docker search 镜像名称
### 22. 创建一个python运行环境的容器
#### 1)
    docker run -d -P training/webapp python app.py
#### 2)
    docker run -d -p 5000:5000 training/webapp python app.py
#### 端口绑定指令说明
    -P :大写；是容器内部端口随机映射到主机的端口。
    -p :小写；是容器内部端口绑定到指定的主机端口。
#### 23. 指定容器绑定的网络地址
    docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
#### 24. 绑定 UDP 端口
    docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
>查看单个命令的详细用法

    docker 命令 --help
## 设置`Docker`容器中的某一个软件为默认的进程，然后显示版本号
### 1. 新增`Dockerfile`文件
    nano Dockerfile
### 2. 复制粘贴以下的配置文件并按照描述修改
    FROM 镜像名称
    
    ENTRYPOINT ["软件名"]
    CMD ["软件对应的指令"]
#### 例
    FROM node:latest
    
    ENTRYPOINT ["node"]
    CMD ["-v"]
## 创建镜像
### 1. 先拉取一个官方的或别人的镜像
### 2. 运行一个容器
### 3. 在容器中添加或部署你的应用或环境
### 4. 通过命令提交容器副本
    docker commit -m = "提交的描述信息" -a = " 镜像作者 " 容器名称ID 新创建镜像的名称
#### 例
    docker commit -m = "has update" -a = " runoob " e218edb10161 runoob / ubuntu : v2
### 5. 用新镜像启动一个容器
## 构建镜像
#### 在 Dockerfile 文件的存放目录下，执行构建动作。
### 1. 新建`Dockerfile`文件，告诉 Docker 如何构建我们的镜像
    FROM    centos:6.7
    MAINTAINER      Fisher "fisher@sudops.com"
    
    RUN     /bin/echo 'root:123456' |chpasswd
    RUN     useradd runoob
    RUN     /bin/echo 'runoob:123456' |chpasswd
    RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
    EXPOSE  22
    EXPOSE  80
    CMD     /usr/sbin/sshd -D
#### 每一个指令都会在镜像上创建一个新的层，
#### 每一个指令的前缀都必须是大写的。
#### 第一条FROM，指定使用哪个镜像源
#### RUN 指令告诉docker 在镜像内执行命令，安装了什么。。。
### 2. 使用 Dockerfile 文件，通过 docker build 命令来构建一个镜像
#### 以下示例，通过目录下的 Dockerfile 构建一个 nginx:v3（镜像名称:镜像标签）。
    docker build -t runoob/centos:6.7 .
#### 参数说明
    -t ：指定要创建的目标镜像名
    . ：代表本次执行的上下文路径
#### 上下文路径，是指 docker 在构建镜像，有时候想要使用到本机的文件（比如复制），docker build 命令得知这个路径后，会将路径下的所有内容打包
#### 解析：由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。实际的构建过程是在 docker 引擎下完成的，所以这个时候无法用到我们本机的文件。这就需要把我们本机的指定目录下的文件一起打包提供给 docker 引擎使用。
#### 如果未说明最后一个参数，那么默认上下文路径就是 Dockerfile 所在的位置。
#### 注意：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢
## 设置镜像标签
    docker tag 容器ID 镜像名称:新标签名
#### 例
    docker tag 860c279d2fec runoob/centos:dev
## 镜像推送
### 1. 以下命令中的 username 请替换为你的 Docker 账号用户名
    docker tag ubuntu:18.04 username/ubuntu:18.04
### 2. 将自己的镜像推送到 Docker Hub
    docker push username/ubuntu:18.04
## Docker容器互联
### 1. 新建网络
    docker network create -d bridge 网络容器名称
#### 例
    docker network create -d bridge test-net
#### 参数说明
    -d：参数指定 Docker 网络类型，有 bridge、overlay。
    其中 overlay 网络类型用于 Swarm mode，在本小节中你可以忽略它
### 2. 运行一个容器并且连接到`test-net`网络中
    docker run -itd --name test1 --network test-net ubuntu /bin/bash
### 3. 打开新的终端，再运行一个容器并加入到`test-net`网络
    docker run -itd --name test2 --network test-net ubuntu /bin/bash
### 4. 给容器安装ping命令(如果没有；先要进入容器更新容器的系统)
    apt install iputils-ping
### 5. 在宿主机的 /etc/docker/daemon.json 文件中增加以下内容来设置全部容器的 DNS(需要重启 docker 才能生效)
    {
        "dns" : [
            "114.114.114.114",
            "8.8.8.8"
        ]
    }
### 6. 查看容器的 DNS 是否生效
    docker run -it --rm  ubuntu  cat etc/resolv.conf
### 7. 在指定的容器设置 DNS
    docker run -it --rm -h host_ubuntu  --dns=114.114.114.114 --dns-search=test.com ubuntu
#### 参数说明：
    --rm：容器退出时自动清理容器内部的文件系统。
    -h HOSTNAME 或者 --hostname=HOSTNAME： 设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。
    --dns=IP_ADDRESS： 添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。
    --dns-search=DOMAIN： 设定容器的搜索域，当设定搜索域为 .example.com 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 host.example.com
## Dockerfile
### 1. FROM：定制的镜像都是基于 FROM 的镜像，这里的 nginx 就是定制需要的基础镜像。后续的操作都是基于 nginx。

### 2. RUN：用于执行后面跟着的命令行命令。有以下俩种格式：
#### shell 格式：
    RUN <命令行命令>
    # <命令行命令> 等同于，在终端操作的 shell 命令。
#### exec 格式：
    RUN ["可执行文件", "参数1", "参数2"]
    # 例如：
    # RUN ["./test.php", "dev", "offline"] 等价于 RUN ./test.php dev offline
### 3. 注意：Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。例如：
    FROM centos
    RUN yum -y install wget
    RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
    RUN tar -xvf redis.tar.gz
#### 以 && 符号连接命令，这样执行后，只会创建 1 层镜像：
    FROM centos
    RUN yum -y install wget \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && tar -xvf redis.tar.gz
### 4. 指令详解
#### COPY
#### 复制指令，从上下文目录中复制文件或者目录到容器里指定路径。
#### 格式：
    COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
    COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]
## Docker Compose
### 1. 独立下载和安装`Docker Compose`
    curl -SL https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
#### 如果`docker-compose`命令安装失败，请检查您的路径
### 2. 授权下载的`docker-compose`文件
    chmod +x /usr/local/bin/docker-compose
### 3. 在路径中创建指向或任何其他目录的符号链接
    ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
### 4. 查看版本
    docker compose version
### 5. 在 app 项目的根目录创建一个名为`Dockerfile`的文件，内容如下：
    FROM python:3.7-alpine
    WORKDIR /code
    ENV FLASK_APP app.py
    ENV FLASK_RUN_HOST 0.0.0.0
    RUN apk add --no-cache gcc musl-dev linux-headers
    COPY requirements.txt requirements.txt
    RUN pip install -r requirements.txt
    COPY . .
    CMD ["flask", "run"]
#### Dockerfile 内容解释：
##### FROM python:3.7-alpine: 从 Python 3.7 映像开始构建镜像。
##### WORKDIR /code: 将工作目录设置为 /code。
##### 设置 flask 命令使用的环境变量。
    ENV FLASK_APP app.py
    ENV FLASK_RUN_HOST 0.0.0.0
##### RUN apk add --no-cache gcc musl-dev linux-headers: 安装 gcc，以便诸如 MarkupSafe 和 SQLAlchemy 之类的 Python 包可以编译加速。
##### 复制 requirements.txt 并安装 Python 依赖项
    COPY requirements.txt requirements.txt
    RUN pip install -r requirements.txt
##### COPY . .: 将 . 项目中的当前目录复制到 . 镜像中的工作目录。
##### CMD ["flask", "run"]: 容器提供默认的执行命令为：flask run。
### 6. 在` app `项目的根目录创建一个名为 docker-compose.yml 的文件，然后粘贴以下内容
    # yaml 配置
    version: '3'
    services:
        web:
            build: .
            ports:
            - "5000:5000"
        redis:
            image: "redis:alpine"
#### 首先定义版本
    version: "3.7"
#### 定义希望作为应用程序的一部分运行的服务（或容器）列表
    version: "3.7"
    
    services:
#### 定义了两个服务：web 和 redis。
#### `web：该 web 服务使用从 Dockerfile 当前目录中构建的镜像。然后，它将容器和主机绑定到暴露的端口 5000。此示例服务使用 Flask Web 服务器的默认端口 5000 `
#### redis：该 redis 服务使用 Docker Hub 的公共 Redis 映像。
### 7.在`app`项目目录中，执行以下命令来启动应用程序
    docker-compose up -d
## Docker Machine 在虚拟主机上安装 Docker 的工具
## 安装`Docker Machine`
#### linux
    curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
    chmod +x /tmp/docker-machine &&
    sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
#### windows
    base=https://github.com/docker/machine/releases/download/v0.16.0 && mkdir -p "$HOME/bin" && curl -L $base/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" && chmod +x "$HOME/bin/docker-machine.exe"
#### 查看`docker-machine`的版本
    docker-machine version
## `Docker Machine`使用
### 1. 列出可用的机器
    docker-machine ls
### 2. 查看是否安装了virtualbox
    which virtualbox
### 3. 查看是否安装了VBoxManage
    which VBoxManage
### 4. 安装virtualbox(如果没有)
#### Debian / ubuntu
    apt install -y virtualbox
#### Cent OS
    yum install -y virtualbox
### 5. 创建一台名为 test 的机器
    docker-machine create --driver 机器的驱动类型 机器名称
#### 例
    docker-machine create --driver virtualbox test
### 6. 查询机器的IP
    docker-machine ip 机器名称
### 7. 停止机器
    docker-machine stop 机器名称
### 8. 启动机器
    docker-machine start 机器名称
### 9. 进入机器
    docker-machine ssh 机器名称
### 10. 查看激活状态的的`docker主机`
    docker-machine active
## 集群管理
### 1. 创建 swarm 集群管理节点（manager）
    docker-machine create -d virtualbox swarm-manager
### 2. 初始化 swarm 集群，进行初始化的这台机器，就是集群的管理节点(先进入机器，再执行这条命令)
    docker swarm init --advertise-addr 192.168.99.107 #这里的 IP 为创建机器时分配的 ip
### 3. 初始化成功，在输出的信息中需要把以下这行复制出来，在增加工作节点时会用到
    docker swarm join --token SWMTKN-1-4oogo9qziq768dma0uh3j0z0m5twlm10iynvz7ixza96k6jh9p-ajkb6w7qd06y1e33yrgko64sk 192.168.99.107:2377
### 4. 创建 swarm 集群工作节点(worker)
#### 先列出可用的机器，可以看见已经创建好俩台机器，swarm-worker1 和 swarm-worker2(如果没有请创建它们) 
### 5. 分别进入两个机器里，指定添加至上一步中创建的集群，这里会用到上一步复制的内容
    docker swarm join --token SWMTKN-1-4oogo9qziq768dma0uh3j0z0m5twlm10iynvz7ixza96k6jh9p-ajkb6w7qd06y1e33yrgko64sk 192.168.99.107:2377
### 6. 进入管理节点，查看当前集群的信息
    docker info
#### 注意：跟集群管理有关的任何操作，都是在管理节点上操作的。
### 7. 在一个工作节点上创建一个名为 helloworld 的服务，这里是随机指派给一个工作节点
    docker service create --replicas 1 --name helloworld alpine ping docker.com
### 8. 查看 helloworld 服务运行在哪个节点上
    docker service ps helloworld
### 9. 查看 helloworld 部署的具体信息
    docker service inspect --pretty helloworld
### 10. 将上述的 helloworld 服务扩展到俩个节点
    docker service scale helloworld=2
### 11. 删除服务
    docker service rm helloworld
### 12. 滚动升级服务
#### 创建一个 3.0.6 版本的 redis
    docker service create --replicas 1 --name redis --update-delay 10s redis:3.0.6
### 13. 滚动升级 redis
    docker service update --image redis:3.0.7 redis
### 14. 查看redis的版本
    docker service ps redis
### 15. 查看所有的节点
    docker node ls
### 16. 停止节点 swarm-worker1
    docker node update --availability drain swarm-worker1
### 17. 启用节点 swarm-worker1
    docker node update --availability active swarm-worker1