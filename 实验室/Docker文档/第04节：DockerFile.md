## 一、DockerFile常用指令

### 1、FROM 

拉镜像

```dockerfile
FROM 镜像名:TAG
FROM 镜像名@digest[效验码]
```



### 2、MAINTAINER

作者描述

还常用LABEL

```bash
LABEL "..."
```



### 3、ENV

环境变量设置

```dockerfile
ENV JAVAHOME /usr/local/jdk
```

（1）查看镜像或容器环境变量

```bash
docker inspect 镜像名称/ID(容器内名称/ID)
```

（2）创建容器时添加或指定环境变量

```bash
docker run --env <key>=<value> 镜像名称/ID 
```



### 4、USER

切换使用者身份，默认`root`



### 5、WORKDIR

改变工作目录

默认目录是`/`

```bash
WORKDIR 路径
```



### 6、RUN

用于执行命令（不可执行cd命令）

格式一：Shell格式

```bash
RUN 命令
```

格式二：exec格式

```bash
RUN ['可执行命令',"参数1","参数2",...]
```



### 7、EXPOSE

为容器打开指定要监听的端口，默认tcp协议

```dockerfile
EXPOSE 端口号/协议
```

```dockerfile
EXPOSE 80/tcp 8080/udp
```



### 8、COPY

复制文件（宿主机->镜像）

```bash
COPY 宿主机路径 镜像路径
```

宿主机路径可为绝对和相对路径，镜像路径必须为绝对路径



### 9、ADD

和COPY类似



## 二、构建

编写好`dockerfile`文件后构建，文件名为`Dockerfile`

构建命令:

```bash
docker build -t 镜像名称 Dockerfile文件路径
```









