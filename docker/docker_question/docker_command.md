



### 1.镜像相关:

**1.如何批量清理临时镜像文件？**

```
可以使用sudo docker rmi $(sudo docker images -q -f danging=true)命令
```



**2.如何查看镜像支持的环境变量？**

```
使用sudo docker run IMAGE env
```



**3.本地的镜像文件都存放在哪里**

```
于Docker相关的本地资源存放在/var/lib/docker/目录下，其中container目录存放容器信息，graph目录存放镜像信息，aufs目录下存放具体的镜像底层文件。
```



**4.构建Docker镜像应该遵循哪些原则？**

```
整体远侧上，尽量保持镜像功能的明确和内容的精简，要点包括： 
# 尽量选取满足需求但较小的基础系统镜像，建议选择debian:wheezy镜像，仅有86MB大小 
# 清理编译生成文件、安装包的缓存等临时文件 
# 安装各个软件时候要指定准确的版本号，并避免引入不需要的依赖 
# 从安全的角度考虑，应用尽量使用系统的库和依赖 
# 使用Dockerfile创建镜像时候要添加.dockerignore文件或使用干净的工作目录
```





### 2.容器相关:

**1.容器退出后，通过docker ps 命令查看不到，数据会丢失么？**

```
容器退出后会处于终止（exited）状态，此时可以通过 docker ps -a 查看，其中数据不会丢失，还可以通过docker start 来启动，只有删除容器才会清除数据。
```



**2.如何停止所有正在运行的容器？**

```
使用docker kill $(sudo docker ps -q)
```



**3.如何清理批量后台停止的容器**

```
使用docker rm $（sudo docker ps -a -q）
```



**4.如何临时退出一个正在交互的容器的终端，而不终止它？**

```
按Ctrl+p，后按Ctrl+q，如果按Ctrl+c会使容器内的应用进程终止，进而会使容器终止。
```



**5.很多应用容器都是默认后台运行的，怎么查看它们的输出和日志信息？**

```
使用docker logs，后面跟容器的名称或者ID信息
```



**6.使用docker port 命令映射容器的端口时，系统报错Error: No public port ‘80’ published for …，是什么意思？**

```
创建镜像时Dockerfile要指定正确的EXPOSE的端口，容器启动时指定PublishAllport=true
```



**7.可以在一个容器中同时运行多个应用进程吗？**

```
一般不推荐在同一个容器内运行多个应用进程，如果有类似需求，可以通过额外的进程管理机制，比如supervisord来管理所运行的进程
```



### 3.仓库相关：

**1.仓库（Repository）、注册服务器（Registry）、注册索引（Index）有何关系？**

```
首先，仓库是存放一组关联镜像的集合，比如同一个应用的不同版本的镜像，注册服务器是存放实际的镜像的地方，注册索引则负责维护用户的账号，权限，搜索，标签等管理。注册服务器利用注册索引来实现认证等管理。
```



**2 、从非官方仓库（如：dl.dockerpool.com）下载镜像的时候，有时候会提示“Error：Invaild registry endpoint <https://dl.docker.com:5000/v1/>…”?**

```
Docker 自1.3.0版本往后以来，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。 
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000” 
重启docker服务
```



### 4.配置相关:

**1、Docker的配置文件放在那里。如何修改配置？**

```
Ubuntu系统下Docker的配置文件是/etc/default/docker，CentOS系统配置文件存放在/etc/sysconfig/docker
```



**2. Docker 修改默认存储路径的一个方法**

```
2. 为了解决这个问题, 计划将docker的默认存储路径从/var/lib/docker中移出去
方法: 在/home 目录下创建目录.
cd /home
mkdir docker
3. 修改docker的systemd的 docker.service的配置文件
不知道 配置文件在哪里可以使用systemd 命令显示一下.
systemctl disable docker
systemctl enable docker
#显示结果
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
4. 修改docker.service文件.
vim /usr/lib/systemd/system/docker.service
5. 在里面的EXECStart的后面增加后如下:
ExecStart=/usr/bin/dockerd --graph /home/docker
6. 重新enable 一下docker 服务 重新进行软连接 以及进行一次 daemon-reload
systemctl disable docker
systemctl enable docker
systemctl daemon-reload
systemctl start docker
```





### 5.docker与虚拟化:

**1. Docker与LXC（Linux Container）有何不同？**

```
LXC利用Linux上相关技术实现容器，Docker则在如下的几个方面进行了改进：
移植性：通过抽象容器配置，容器可以实现一个平台移植到另一个平台； 
镜像系统：基于AUFS的镜像系统为容器的分发带来了很多的便利，同时共同的镜像层只需要存储一份，实现高效率的存储； 
版本管理：类似于GIT的版本管理理念，用户可以更方面的创建、管理镜像文件； 
仓库系统：仓库系统大大降低了镜像的分发和管理的成本； 
周边工具：各种现有的工具（配置管理、云平台）对Docker的支持，以及基于Docker的Pass、CI等系统，让Docker的应用更加方便和多样化。
```



**2. Docker与Vagrant有何不同？**

```
两者的定位完全不同 
Vagrant类似于Boot2Docker（一款运行Docker的最小内核），是一套虚拟机的管理环境，Vagrant可以在多种系统上和虚拟机软件中运行，可以在Windows。Mac等非Linux平台上为Docker支持，自身具有较好的包装性和移植性。 
原生Docker自身只能运行在Linux平台上，但启动和运行的性能都比虚拟机要快，往往更适合快速开发和部署应用的场景。
```



**3.如何将一台宿主机的docker环境迁移到另外一台宿主机？**

```
停止Docker服务，将整个docker存储文件复制到另外一台宿主机上，然后调整另外一台宿主机的配置即可
```
