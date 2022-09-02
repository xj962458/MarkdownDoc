## 一、介绍

​		`Docker`最重要的两个概念，一个是镜像，另一个就是容器。和虚拟机对比，可以将镜像理解为系统的安装包，容器就是安装好的系统，与虚拟机不同的是，`Docker`由镜像创建容器非常的快速，一般都在秒级，而虚拟机通过系统镜像安装虚拟机则需要分钟级或更长的时间。以下将介绍镜像和容器的一些操作命令，通过这些命令，就可以基本掌握`Docker`的使用，并利用`Docker`开发一些有趣的项目。

​		`Docker`的知识不仅包含基本的操作命令，还有`Docker Compose`、`DockerFile`和`Kubernetes`等，但`Docker`的基本命令是最基础的东西，学的足够熟练了，才能够更好地掌握其他更高阶的知识。此处只介绍了`Docker`最常用的镜像操作和容器操作命令，此外还有卷操作、网络操作等命令，但用的较少，此处不多介绍，需要的可以自行了解。

​	

## 二、镜像管理

### 1、`DockerHub`介绍与使用
​        `docker`镜像默认是从`DockerHub`上拉取的，也可以从其它地方拉取，不过要进行配置。

​		`DockerHub`的官方地址为：[https://hub.docker.com](https://hub.docker.com/)
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709193640.png)
如图所示，可以搜索所需镜像，还可以进行筛选

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709194459.png)

点击一个镜像进入，在其右侧可以看到拉取镜像的命令，而`Description`、`Reviews`和`Tags`则是镜像的描述、评论和版本介绍。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709194508.png)



### 2、登陆

```bash
docker login [选项] [服务器]
```

选项：

| 选项             | 含义            |
| ---------------- | --------------- |
| --password,-p    | 密码            |
| --password-stdin | 从stdin获取密码 |
| --username,-u    | 用户名          |

服务器：

腾讯云：`ccr.ccs.tencentyun.com`

阿里云：`registry.cn-hangzhou.aliyuncs.com`



### 3、镜像加速

拉取镜像时默认使用国外的镜像源拉取，在国内速度可能比较慢，可以换国内源，以下以阿里云为例。但国内的镜像源版本可能较老，所以若使用更新版本的镜像，不要换源，不换源的速度还算可以。

Linux教程如下，其他系统不再介绍

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://5at5pajt.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



### 4、拉取镜像

```bash
docker pull 镜像名:TAG
```
​       此命令会从DockerHub拉取镜像，`TAG`是镜像的版本，默认为`latest`，即最新版，镜像一般是分层创建的，所以拉取的时候也是一步步分层进行拉取的，最后整合成一个镜像。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709174024.png)

拉取镜像用到的不是很多，因为在创建容器时，若本地不存在指定的镜像，那么`Docker`就会自动进行拉取。



### 4、查看镜像

```bash
docker images
```
如下图所示，可以列出本地存在的所有镜像，列出的信息含义如下：

- **REPOSITORY：**表示镜像的仓库源，换言之就是镜像名称；
- **TAG：**镜像的标签，可理解为版本；
- **IMAGE ID：**镜像ID（唯一）；
- **CREATED：**镜像创建时间；
- **SIZE：**镜像占用空间大小。![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709174123.png)

​        **镜像ID是镜像实际ID的一部分截取，所以如图所示，`7e7e458be53`这个ID可以代表`mysql:latest`这个镜像，`7e7e45`也可以代表这个镜像，只要能够区别其他镜像的镜像ID都是有效的，之后的容器ID也是同理。**



### 5、删除镜像

```bash
docker rmi 镜像名:TAG/镜像I
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709182308.png)

需要注意的是，若存在使用此镜像创建的容器，则删除该镜像报错，需要先移除创建的容器，再对镜像进行删除。



### 6、镜像重命名

```bash
docker tag 镜像名:TAG/镜像ID 新镜像名:新TAG
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710084501.png)



### 7、利用镜像创建容器

​       根据镜像可以创建容器，可以创建后运行，也可以只创建不运行。如果不指定一个镜像的版本标签，例如只使用`ubuntu`，`docker` 将默认使用`ubuntu:latest`镜像。一般`run`命令用的较多，而`create`命令较少使用，因为一般创建容器就需要它运行从而实现功能。
```bash
docker run 镜像名称:TAG/镜像ID     # 创建并运行
docker create 镜像名称:TAG/镜像ID  # 只创建不运行
```

因需求不同，在创建容器时，需要指定一些参数，以实现一些功能，创建容器时常用的参数如下：

| 名称                |                       说明                       |
| :------------------ | :----------------------------------------------: |
| -i                  |                    交互式操作                    |
| -t                  | 为容器重新分配一个伪输入终端，通常与 -i 同时使用 |
| -d                  |            后台运行容器，并返回容器ID            |
| --name="ubuntu"     |                   指定容器名称                   |
| --rm                |                退出时自动删除容器                |
| --privileged=true   |                   启用特级权限                   |
| --restart           |  指定重启策略，默认为no，可设置为true或always等  |
| --port，-p          |  指定端口映射，格式为：主机(宿主)端口:容器端口   |
| --volume，-v        |                      绑定卷                      |
| -e hostname="linux" |                   环境变量设置                   |

#### ① 创建一个前台ubuntu容器

```bash
docker run -it --name="ubuntu" ubuntu bash 
```

#### ② 创建一个后台运行的ubuntu容器

```bash
docker run -itd --name="ubuntu" ubuntu bash
```

#### ③ 将ubuntu容器的80端口映射到主机的88端口

```basj
docker run -itd -p 88:80 ubuntu bash
```

#### ④ 指定主机名称

```bash
docker run -it -e hostname="linux" ubuntu bash 
```

#### ⑤ 设置容器退出后自动删除

```bash
docker run -it --rm=true ubuntu bash
```

#### ⑥ 设置容器随docker启动而启动

```bash
docker run -itd --restart=always ubuntu bash
```

#### ⑦ 设置卷映射

```bash
docker run -itd -v $PWD/test:/home/test ubuntu bash
```

将当前目录下的test文件映射到容器中的`/home/test`目录，这两个文件夹同步修改，即使删除容器，宿主机上的该文件夹依然存在。

#### ⑧ 创建特权模式的容器

```bash
docker run -itd --name="centos" -p 2222:22 --privileged=true centos:7 /sbin/init
```

​         特权模式的容器才拥有真正的`root`权限，可以使用`systemctl`等命令，在使用ssh时会用到，但使用官方的ubuntu镜像创建容器无法开启特权模式，但使用`centos`官方镜像可以成功创建。



### 8、镜像的导出

​       镜像的导出，顾名思义，就是将镜像保存成一个文件，一般为`tar`或`tag.gz`文件，保存成文件后就可以随便复制，重新导入后不需要网络就可以在不同计算机上使用。

```bash
docker save -o 文件名.tar 镜像名称:TAG/镜像ID
```
建议使用镜像名称:TAG导出，使用镜像ID导出，再导入时会出现镜像`REPOSITORY` 和`TAG`均为`<none>`的情况。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710090047.png)



### 9、镜像的导入

使用`save`文件导出的文件，可以复制到其他计算机上，从文件还原镜像可以使用`load`命令还原。

```bash
docker load [参数]
```
- **--input , -i :** 指定导入的文件；
- **--quiet , -q :** 精简输出信息。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710090425.png)
如上图，使用以下命令导入结果一样:
```bash
docker load < ubuntu.tar
docker load -i ubuntu.tar
```



### 10、查看镜像制作过程

```bash
docker history 镜像名称
```



## 二、容器管理

### 1、查看创建的容器
```bash
docker ps -a
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709183355.png)
如上图所示，可以列出所有容器，列出的信息含义如下：

* **CONTAINER ID**：容器ID，唯一，创建、启动和删除容器时都会用到；

* **IMAGE**：创建容器的镜像名称；    

* **COMMAND**：创建容器时在容器内运行的命令 ；                

* **CREATED**：创建时间；         

* **STATUS**：当前容器状态；                     

* **PORTS**：容器的端口映射情况；     

* **NAMES**：容器名称，若指定，则为指定的，若未指定，则系统随机生成。

  


### 2、退出容器

​       使用`docker run`命令运行容器时，若不加`-d`命令，创建成功后，就会进入容器的终端，要退出该终端只需要执行`exit`命令即可。初期除此之外，使用`exec`命令进入容器后退出容器也是使用`exit`命令退出。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709182926.png)



### 3、启动和停止容器

当不在后台运行容器时，退出容器容器会关闭，若想再次启用容器，需要重新开启
```bash
docker start 容器ID/容器名称
```

正在运行的容器若停止，可使用以下命令

```bash
docker stop 容器ID/容器名称
```



### 4、进入容器

​       创建容器时，若指定容器后台运行，则不会出现容器终端，若想进入容器终端，可以使用以下命令，除此之外，还可以使用`attach`命令，但使用该命令进入容器，退出后容器会停止，所以还是建议使用`exec`命令进入容器。

```bash
docker exec -it 容器ID/容器名称 bash
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709191146.png)
如上图所示，使用容器名称和容器ID均可进入容器



### 5、删除容器
删除容器使用
```bash
docker rm 容器ID/容器名称
```
删除容器前需要先停止容器，然后再删除，若不想停止容器再删除，可使用强制删除：
```bash
docker rm -f 容器ID/容器名称
```
若删除所有容器，可使用以下命令:
```bash
docker rm -f $(docker ps -a -q)
```



### 6、在宿主机和容器之间交换文件

```bash
# 容器->宿主机
docker cp 容器名称/ID:PATH 本地路径

# 宿主机->容器
docker cp 本地路径 容器名称/ID:PATH
```



### 7、查看容器日志

```bash
docker logs [参数] 容器名称/ID
```

参数:

| 参数                 | 含义               |
| -------------------- | ------------------ |
| --since "2022-08-04" | 指定起始日期       |
| -f                   | 查看实时日志       |
| -t                   | 查看日志产生的日期 |
| -tail=n              | 查看最后n条日志    |



### 6、将容器打包成镜像

① 创建一个容器

```bash
docker run -itd --name="ubuntu_new" ubuntu bash
```

② 进入容器

```bash
docker exec -it ubuntu_new bash
```

③ 进行一系列操作

可以给容器添加新的功能，比如，我现在给安装一个`neofetch`软件

```bash
apt update -y && apt install neofetch -y
```

④ 退出容器

```
exit
```

⑤ 打包

```bash
docker commit -m="add neofetch" -a="new" ubuntu_new my_ubuntu:alpha
```

- **-m** ：提交的描述信息；
- **-a**： 指定镜像作者；
- **ubuntu_new**：容器名称，也可以用容器ID，可自定义；
- **my_ubuntu:alpha** ：新镜像的名称和标签，可自定义；

再次运行

```bash
docker images
```

就可以看见新创建的镜像，通过该镜像，可以创建新容器，新的容器拥有已添加的功能。



### 7、将容器打包成文件

```bash
docker export -o 文件名称.tar 容器名称/容器ID
```

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710091213.png)



### 8、从打包好的容器文件中创建镜像

​        使用`export`将容器打包成文件，可以还原，但是还原不能够还原成容器了，只能够还原成镜像，然后再通过镜像创建新的容器。将打包好的容器文件还原成镜像，不能使用`load`命令，需要使用和`export`对应的`import`命令。
```bash
docker import 文件名称 新镜像名:新镜像TAG
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710091832.png)



​		除此之外，`Docker`还有很多其他的命令，但并不常用，所以此处不多介绍，感兴趣的可以自行查阅。接下来一节，介绍以上学习的命令，去搭建一些实用的容器，以实现一些功能。



## 三、数据卷

数据卷(volume)是一个可供一个或多个容器使用的（共享&独立）特殊目录

### 1、创建数据卷

```bash
docker volume create 数据卷名称
```

默认路径：`/var/lib/docker/volume/数据卷名称/_data`



### 2、查看数据卷

```bash
docker volume inspect 数据卷名称
```



### 3、查看所有数据卷信息

```bash
docker volume ls
```



### 4、删除数据卷

```bash
docker volume rm 数据卷名称
```



### 5、应用数据卷

#### （1）当映射数据卷时，如果数据卷不存在，`docker`会自动创建

```bash
docker run -v 数据卷名称:容器内路径 镜像名称/ID
```



#### （2）直接指定一个路径作为数据卷的存储位置

```bash
docker run -v 宿主机路径:容器内路径 镜像名称/ID
```

