---
title: 12-Docker_Notebook
tags:
  - null
  - null
cover: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/wallhaven-wqgr9x_1920x1080.png?raw=true
top_img: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/default_top_img.png?raw=true
date: 2022-08-01 10:45:38
description:
---

# Docker

## Docker能做什么

将**应用、依赖包**打包到一个容器中，然后发布到流星的Linux机器上，也可以实现虚拟化。

**应用场景**：

- Web应用的自动化打包和发布；
- 自动化测试和持续集成、发布；
- 在服务型环境中部署和调整数据库或其他后台应用；
- 从头编译或扩展现有的OpenShift或Cloud Foundry平台搭建自己的PaaS环境；

## 常用的命令

- 使用`docker run`命令在容器内运行一个应用程序（创建一个容器）

```dockerfile
~ docker run ubuntu:15.10 /bin/echo "Hello world"
Hello world
```



- 使用`-i`、`-t`参数，让docker运行的容器实现对话能力

```dockerfile
~ docker run -i -t ubuntu:15.10 /bin/bash
root@1b658ae35706:/# 
```

其中：

**-i**：允许对容器内的标准输入（STDIN）进行交互

**-t**：在新容器内指定一个伪终端或者终端

**root@1b658ae35706:/#**：进入了一个ubuntu:15.10系统的容器



-  使用`cat /proc/version`查看当前系统版本信息

```dockerfile
root@1b658ae35706:/# cat /proc/version
Linux version 5.10.47-linuxkit (root@buildkitsandbox) (gcc (Alpine 10.2.1_pre1) 10.2.1 20201203, GNU ld (GNU Binutils) 2.35.2) #1 SMP Sat Jul 3 21:51:47 UTC 2021
```

- 使用`ls`查看当前目录下的文件列表

```dockerfile
root@1b658ae35706:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@1b658ae35706:/# 
```

- 使用`exit`命令或`CTRL+D`退出容器

```dockerfile
root@1b658ae35706:/# exit
exit
➜  ~ 
```

- 使用`-d`让容器在后台运行；

```dockerfile
➜  ~ docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
449af35382666478c85c772766f3b17c47e6d242343756819ff9fc81261ea880
```

其中：

输出的长字符串叫做**容器ID**，对每个容器来说都是唯一的，可以通过容器ID查看对应的容器发生了什么；

- 使用`docker ps`来查看容器是否在运行

```dockerfile
➜  ~ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
449af3538266   ubuntu:15.10   "/bin/sh -c 'while t…"   19 seconds ago   Up 18 seconds             romantic_chebyshev
```

其中：

**CONTAINER ID:** 容器 ID。

**IMAGE:** 使用的镜像。

**COMMAND:** 启动容器时运行的命令。

**CREATED:** 容器的创建时间。

**STATUS:** 容器状态。

状态有7种：

- created（已创建）
- restarting（重启中）
- running 或 Up（运行中）
- removing（迁移中）
- paused（暂停）
- exited（停止）
- dead（死亡）

**PORTS:** 容器的端口信息和使用的连接类型（tcp\udp）。

**NAMES:** 自动分配的容器名称。

- 使用`-P`将容器内部使用的网络端口随机映射到我们使用的主机上

```dockerfile
➜  ~ docker run -d -P training/webapp python app.py
b97c1571c833b87f41bf67f0292d8c2a7b66fe7d48b9691b1a0fcc2f9523cc5f
➜  ~ docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
b97c1571c833   training/webapp   "python app.py"   13 seconds ago   Up 12 seconds   0.0.0.0:55000->5000/tcp   youthful_jackson
➜  ~ 
```

结果多了PORTS

**PORTS**：端口信息



## 镜像使用中常用的命令

- 使用 `docker images` 来列出本地主机上的镜像

```dockerfile
➜  ~ docker images
REPOSITORY             TAG       IMAGE ID       CREATED         SIZE
test/ubuntu            v1        2d18aec27708   2 hours ago     119MB
ubuntu                 latest    27941809078c   7 weeks ago     77.8MB
debian_with_software   v1        5305f878934c   8 weeks ago     1.61GB
debian                 stable    b7903d928a17   2 months ago    124MB
centos                 latest    5d0da3dc9764   10 months ago   231MB
ubuntu                 15.10     9b9cb95443b5   6 years ago     137MB
training/webapp        latest    6fae60ef3446   7 years ago     349MB
➜  ~ 
```

其中：

**REPOSITORY：**表示镜像的仓库源

**TAG：**镜像的标签

**IMAGE ID：**镜像ID

**CREATED：**镜像创建时间

**SIZE：**镜像大小



- 如果要**使用**版本为15.10的ubuntu系统镜像来运行容器时，命令如下：

```dockerfile
runoob@runoob:~$ docker run -t -i ubuntu:15.10 /bin/bash 
root@d77ccb2e5cca:/#
```

其中：

**-i**: 交互式操作

**-t**: 终端。

**ubuntu:15.10**: 这是指用 ubuntu 15.10 版本镜像为基础来启动容器

**/bin/bash**：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash

如果不指定一个镜像的版本标签，例如只使用 ubuntu，docker，将**默认使用 ubuntu:latest 镜像**。



- 使用`docker pull`获取新镜像方式

```dockerfile
➜  ~ docker pull ubuntu:13.10
13.10: Pulling from library/ubuntu
Image docker.io/library/ubuntu:13.10 uses outdated schema1 manifest format. Please upgrade to a schema2 image for better future compatibility. More information at https://docs.docker.com/registry/spec/deprecated-schema-v1/
a3ed95caeb02: Pull complete 
0d8710fc57fd: Pull complete 
5037c5cd623d: Pull complete 
83b53423b49f: Pull complete 
e9e8bd3b94ab: Pull complete 
7db00e6b6e5e: Pull complete 
Digest: sha256:403105e61e2d540187da20d837b6a6e92efc3eb4337da9c04c191fb5e28c44dc
Status: Downloaded newer image for ubuntu:13.10
docker.io/library/ubuntu:13.10
```



- 使用`docker search`查找镜像

```dockerfile
➜  ~ docker search httpd
NAME                                    DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
httpd                                   The Apache HTTP Server Project                  4101      [OK]       
centos/httpd-24-centos7                 Platform for running Apache httpd 2.4 or bui…   44                   
centos/httpd                                                                            35                   [OK]
solsson/httpd-openidc                   mod_auth_openidc on official httpd image, ve…   2                    [OK]
clearlinux/httpd                        httpd HyperText Transfer Protocol (HTTP) ser…   2                    
hypoport/httpd-cgi                      httpd-cgi                                       2                    [OK]
nnasaki/httpd-ssi                       SSI enabled Apache 2.4 on Alpine Linux          1                    
dockerpinata/httpd                                                                      1                    
jonathanheilmann/httpd-alpine-rewrite   httpd:alpine with enabled mod_rewrite           1                    [OK]
inanimate/httpd-ssl                     A play container with httpd, ssl enabled, an…   1                    [OK]
centos/httpd-24-centos8                                                                 1                    
dariko/httpd-rproxy-ldap                Apache httpd reverse proxy with LDAP authent…   1                    [OK]
manageiq/httpd                          Container with httpd, built on CentOS for Ma…   1                    [OK]
publici/httpd                           httpd:latest                                    1                    [OK]
httpdocker/kubia                                                                        0                    
patrickha/httpd-err                                                                     0                    
e2eteam/httpd                                                                           0                    
amd64/httpd                             The Apache HTTP Server Project                  0                    
manageiq/httpd_configmap_generator      Httpd Configmap Generator                       0                    [OK]
manasip/httpd                                                                           0                    
httpdss/archerysec                      ArcherySec repository                           0                    [OK]
ppc64le/httpd                           The Apache HTTP Server Project                  0                    
paketobuildpacks/httpd                                                                  0                    
19022021/httpd-connection_test          This httpd image will test the connectivity …   0                    
sandeep1988/httpd-new                   httpd-new                                       0                    
➜  ~ 
```

其中：

**NAME:** 镜像仓库源的名称

**DESCRIPTION:** 镜像的描述

**OFFICIAL:** 是否 docker 官方发布

**stars:** 类似 Github 里面的 star，表示点赞、喜欢的意思。

**AUTOMATED:** 自动构建。

NOTE：也可以从[DockerHub网站](https://hub.docker.com/)搜索镜像

- 使用`docker pull`拖取/下载镜像

```dockerfile
➜  ~ docker pull httpd
```

下载完成后使用`docker run`来运行这个镜像

```dockerfile
➜  ~ docker run httpd
```

- 使用`docker rmi`删除镜像

```dockerfile
➜  ~ docker rmi ubuntu:13.10 
Untagged: ubuntu:13.10
Untagged: ubuntu@sha256:403105e61e2d540187da20d837b6a6e92efc3eb4337da9c04c191fb5e28c44dc
Deleted: sha256:7f020f7bf34554411031ec0d4f2ab46a2976dad403e1c26bc21dc1bf4c48c8aa
Deleted: sha256:2aac093d13faafda4d0da3534d30274bcc4e5475b1e126c84b9d670862f5e4ef
Deleted: sha256:c676fe3dd3ceb6442e8b23350de88adc6546a52f75bd92dbb1a789b7c6de0fcf
Deleted: sha256:7c6a37fb8fe6a41aaf7c6c7a2cc3d448c01df026b2056a9f35e490e7bf6285cc
Deleted: sha256:b0e8be8278c28daa541ad564fc91dbea99263caa6e5e68db033061c5e08f0315
Deleted: sha256:78dcbd700c6678a7af4422e0ad516628852973a255526f4b617f33db218e1075
Deleted: sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef
➜  ~ docker images
REPOSITORY             TAG       IMAGE ID       CREATED         SIZE
test/ubuntu            v1        2d18aec27708   3 hours ago     119MB
ubuntu                 latest    27941809078c   7 weeks ago     77.8MB
debian_with_software   v1        5305f878934c   8 weeks ago     1.61GB
debian                 stable    b7903d928a17   2 months ago    124MB
centos                 latest    5d0da3dc9764   10 months ago   231MB
ubuntu                 15.10     9b9cb95443b5   6 years ago     137MB
training/webapp        latest    6fae60ef3446   7 years ago     349MB
```

## 常用的Linux命令

使用`scp`复制文件或者文件夹到服务器

```dockerfile
scp -r ./Desktop/docker huawei:~/lyn
Enter passphrase for key '/Users/wind/.ssh/id_rsa':
Image-Classification-master.zip                                                                                                                                9%  158MB   1.3MB/s   19:17 ETA^
```







