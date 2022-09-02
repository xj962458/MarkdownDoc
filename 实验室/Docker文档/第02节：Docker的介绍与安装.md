## 一、介绍
​        `Docker`是一个开源的应用容器引擎，可以轻松地为任何应用创建一个轻量级、可以自给自足的容器。`Docker`和虚拟机软件很像，可以在一个宿主系统上运行其他系统，但是比虚拟机更为高效，一台普通电脑上运行几十个虚拟机已经是极限了，但运行上百个`Docker`容器却轻轻松松。`Docker`和虚拟机的区别如下：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709155704.png)

​        学习Docker需要理解最基本的三个概念——镜像、容器和仓库。镜像，相当于一个打包好的系统，通过镜像，可以用来创建容器，容器就是让打包好的系统——镜像运行起来，以实现相关的功能。而仓库就是存放很多打包好的系统——镜像的地方，用户可以从仓库中拉取很多系统镜像，从而创建容器。



## 二、安装

**之后的Docker的安装和使用，建议在`root`用户下执行，不然可能会出现一些权限问题**

​       `Docker`有很多种安装方式，这里介绍最简单的一种，通过脚本安装，脚本如下，不分`Linux`发行版，只需要在终端中执行，脚本会自动判别系统的类型，从而进行安装。
```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
若提示没有`curl`，可使用以下命令安装`curl`
```bash
apt update -y && apt install curl -y
```

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709173135.png)

安装完成后，输入`docker`会出现以下内容。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709190038.png)

若不是以上内容，则可能docker未启动，可使用以下命令，启动或重启docker
```bash
systemctl start docker   # 启动docker
systemctl restart docker # 重启docker
```

此外，还有其他与docker启动相关的命令
```bash
systemctl stop docker   # 停止docker
systemctl enabel docker # 设置docker开机自启
```

建议设置开机自启，那样每次开机后就不用重新打开`Docker`了