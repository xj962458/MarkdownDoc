# 一、介绍

​	   **此篇文档是介绍`Nvidia-Docker`，本篇教程需要使用带有英伟达`GPU`的机器，最好是服务器，不适合在普通虚拟机上进行操作，若无带有英伟达显卡的机器，便不用尝试此节的操作。**

​       传统的`docker`可以用来部署各种容器，以在一台设备上运行多个隔离的服务，缺点是并不能够调用`GPU`。最近实验室有台装了`T4`的服务器，想着能不能够装个带`GPU`的`docker`容器，然后远程连接用来训练我的模型。查了一下，其实是可以的，不过要借助`nvidia-docker`这个工具，经过了一番折腾，算是成功了，现将折腾的过程记录如下。

​        要想使用`nvidia-docker`创建带`GPU`的容器，需要安装英伟达显卡的驱动，网上看很多教程，说还要安装`cuda`和`cudnn`，其实这两个在宿主机上不安装也行，等创建容器的后，在容器内部安装配置即可。而且不同的容器可以安装配置不同的`cuda`和`cudnn`，创建容器之后，只要将其打包成镜像，那么用不同版本的`cuda`时只要创建个容器，将会非常的方便。

**官方nvidia-docker文档：**[https://docs.nvidia.com/datacenter/cloud-native/](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#)



# 二、CentOS7服务器部署nvidia-Docker

## 1、安装`NVIDIA`显卡驱动
​        可以去官方网站下载，根据自己的显卡选择对应版本，最新的也可以（我安装的就是最新的），不影响之后`docker`容器内的显卡使用。可以先在个人电脑上下载，然后利用`FTP`或`SFTP`传到服务器上，也可以复制下载链接后在服务器中使用`wget`命令进行下载：

```bash
wget https://cn.download.nvidia.cn/tesla/510.47.03/NVIDIA-Linux-x86_64-510.47.03.run
```

没有`wget`命令可进行安装：

```bash
sudo yum install wget -y
```

`NVIDIA`显卡驱动下载地址：[https://www.nvidia.cn/Download/index.aspx?lang=cn](https://www.nvidia.cn/Download/index.aspx?lang=cn)
根据显卡型号和对应的平台就可以进行下载
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185100.png)
以我下载的驱动为例，我下载后的文件如下：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185128.png)
&nbsp;    如上图所示，得到下载的文件后，先使用`chmod +x`赋予文件可执行权限，然后执行程序，一路按默认的来，安装即可。



## 2、安装`docker`

**这里使用阿里云一键安装脚本进行安装，安装docker后记得开启docker并设置开机自启动**
```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

```bash
systemctl start docker    #开启docker
systemctl enable socker  #设置开机自启
```

更多安装方法可见：[Linux安装docker](https://blog.csdn.net/qq_29183811/article/details/114739672)



## 3、设置`nvidia-docker`的存储库和 GPG 密钥

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.repo | sudo tee /etc/yum.repos.d/nvidia-docker.repo
```



## 4、更新包列表后安装包和依赖项

```bash
sudo yum clean expire-cache
```



## 5、安装`nvidia-docker`

&nbsp; &nbsp; &nbsp; &nbsp;`nvidia-docker1`已被弃用，可直接安装`nvidia-docker2`，使用和`nvidia-docker`一样：

```bash
sudo yum install -y nvidia-docker2
```
然后重启`docker`

```bash
sudo systemctl restart docker
```



## 6、测试是否安装成功

```bash
sudo nvidia-docker run --rm --gpus all nvidia/cuda:11.2-base nvidia-smi
```
输出结果如下:
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185231.png)


## 7、总结

​       安装`nvidia-docker`其实就三步，安装`NVIDIA`显卡驱动，安装`docker`和安装`nvidia-docker`，服务器可以不安装`cuda`和`cudnn`。



# 三、使用nvidia-docker搭建深度学习容器

​        安装了`nvidia-docker`，就该介绍如何使用`nvidia-docker`创建`GPU`容器，这样创建的容器不会影响宿主机的开发环境，还可以创建多台容器，使用多个`cuda`版本，多人同时使用，极大地提高了开发效率。


## 1、拉取镜像
&nbsp; &nbsp; &nbsp; &nbsp;和`nvidai-docker`相切合的`docker`镜像是`nvidia/cuda`，`Dockerhub`的地址为[nvidia/cuda](https://registry.hub.docker.com/r/nvidia/cuda/tags)
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185256.png)
这里的镜像有很多类型，最基本的三种类型如下：
>  - base：基于cuda，包含最精简的依赖，用于部署预编译的cuda应用，需要手工安装所需依赖
>  - runtime：基于base，添加了cuda tookit共享库
>  - devel：基于runtime，添加了编译工具链、调试工具、头文件、静态库，用于从源码编译cuda应用

&nbsp; &nbsp; &nbsp; &nbsp;除此之外，还有基于各种系统生成的镜像，有`Centos`、`ubuntu`和`ubi`系统的，基于`ubuntu`系统编译的镜像无法使用特权模式，即无法正常使用`systemctl`等命令，而`centos8`停服了，所以我选择了我比较熟悉的`centos7`系统。
&nbsp; &nbsp; &nbsp; &nbsp;另外还有带`cudnn`的版本，如`11.5.0-cudnn8-runtime-ubuntu20.04`，里面包含了`cudnn`，但是呢，他的`cudnn`没有和`cuda`放一起，进去还得单独配置，有点麻烦，所以我选择了没有`cudnn`的版本，等部署好容器后，我自己自行配置`cudnn`。
&nbsp; &nbsp; &nbsp; &nbsp;所以，扯皮扯了半天，我用的哪个版本？我主要用`PaddlePaddle`和`Pytorch`，所以我装的`cuda10.2`的版本，如果使用`tensorflow`的话，这个版本是不行的，`tensorflow`不支持`cuda10.2`，得用`cuda11.2`。我使用的是`nvidia/cuda:11.2.2-devel-centos7`镜像，拉取命令如下，接下来就以此来演示如何部署深度学习容器。

```bash
docker pull nvidia/cuda:11.2.2-devel-centos7
```


## 2、创建容器
命令如下：

```bash
nvidia-docker run -itd --name=DL -p 2222:22 --privileged=true --gpus all nvidia/cuda:10.2-devel-centos7 /usr/sbin/init
```

>  - `-it` 交互模式运行
>  - `-d` 守护进程，人话就是让它在后台跑
>  - `--name` 指定容器名称
>  - `-p` 映射端口号，格式为：**主机端口:容器端口**，这里映射22端口是为了用`SSH`连接
>  - `--privileged=true`  特权模式，为了之后使用`sytemctl`来重启`sshd`
>  - `--gpus all` 指定用的cpu，因为只要一块，就全指定了，其他指定方式自行百度
>  - `/usr/sbin/init` 这个也是为了启动特权模式，换`/bin/bash`就不行



## 3、安装并配置`ssh`
### （1）安装`openssh-server`
```bash
sudo yum update -y && sudo yum install openssh-server -y
```
### （2）修改密码

```bash
passwd
```
输入两次新密码就可以了，记住，用来之后连接容器
### （3）配置公钥
&nbsp; &nbsp; &nbsp; &nbsp;若没有公钥登录需求，可以直接使用密码就可，不用管这一步
#### ①、创建`.ssh`文件夹和`authorized_keys`文件

```bash
mkdir ~/.ssh
touch ~/.ssh/authorized_keys
```

#### ②、修改权限

```bash
cd ~/.ssh
chmod 700 ../
chmod 700 .
chmod 600 authorized_keys
```

#### ③、写入公钥
&nbsp; &nbsp; &nbsp; &nbsp;将自己的公钥写入，那样不用密码就可以登陆了
```bash
vi ~/.ssh/authorized_keys
```

#### ④、重启`sshd`服务（配置文件不用改，配置文件刚好）

```bash
systemctl resatrt sshd 
```
#### ⑤、连接
&nbsp; &nbsp; &nbsp; &nbsp;这么配置后，就可以利用`ssh`连接容器了，刚才容器映射的是`2222`端口，使用一下命令即可以连接

```bash
ssh root@服务器ip -p 2222
```
&nbsp; &nbsp; &nbsp; &nbsp;配置公钥的应该直接就可以连接成功，没配置的就输入密码连接，除了命令行连接，也可以使用	`ssh`工具（如putty、xshell和termius等）进行连接，记得端口是`2222`就行。



## 4、配置`cuda`
&nbsp; &nbsp; &nbsp; &nbsp;容器里面虽然安装了`cuda`，但是`nvcc -V`显示找不到命令，得配置一下环境变量。

```bash
vi /etc/profile.d/cuda.sh
```
将以下内容写入：

```bash
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```
使其生效：

```bash
source /etc/profile
```
这样`nvcc -V`就可以查看`cuda`版本了
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185322.png)

## 5、配置`cudnn`
### （1）下载`cudnn`
&nbsp; &nbsp; &nbsp; &nbsp;`cudnn`的下载需要到官网，而且需要注册英伟达账户，下载地址：[https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)。不知道是不是学校网络原因，我访问非常的慢，科学上网后访问才快了，但是下载文件却可以跑慢速，真的有点奇怪。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185338.png)
&nbsp; &nbsp; &nbsp; &nbsp;如上图所示，根据`cuda`版本选择相应的`cudnn`进行下载。

&nbsp; &nbsp; &nbsp; &nbsp;如下图所示，我的`cuda`版本是10.2，然后我下载第二个（其他满足10.2的也可以），然后选择`Linux`的版本，记得选择`Tar`的包。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185408.png)



### （2）安装`cudnn`

&nbsp; &nbsp; &nbsp; &nbsp;实测，虽然都选择`Tar`的包，但不同版本的下载的包格式不一样，我下载`cuda10.2`版本的`cudnn`是`.tar.xz`格式的包，而下载`cuda11.2`版本的`cudnn`却是`*.tgz`，其他版本的未测，但其实大同小异。
安装`cudnn`的方法也检测，就是将`cudnn`下载包下`lib/lib64`目录下的文件复制到`cuda`的`lib64`目录下
#### ①、如果是`.tar.xz`格式的包
我下载的`cuda10.2`版本的`cudnn`就是这样
解压命令为：

```bash
tar -xvJf *.tar.xz
```
解压后目录结构为：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185443.png)

安装方法：
进入解压的目录，然后执行以下命令：

```bash
cp ./include/* /usr/local/cuda/include/
cp ./lib/* /usr/local/cuda/lib64/
```
&nbsp; &nbsp; &nbsp; &nbsp;网上的教程一般只复制`libcud*`开头的库啥的，但文件也不大，之后估计也能用到，为了简单方便，我就把对应文件全复制过去了

#### ②、如果是`.tgz`格式的包
我下载的`cuda11.2`版本的`cudnn`就是这样
解压命令为：

```bash
tar -zxvf *.tgz
```
解压后目录结构为：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185534.png)
安装方法：
进入解压的目录，然后执行以下命令：
```bash
cp ./include/* /usr/local/cuda/include/
cp ./lib64/* /usr/local/cuda/lib64/
```
<br>
安装完成后，使用一下命令可以查看`cudnn`版本信息：

```bash
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185548.png)
&nbsp; &nbsp; &nbsp; &nbsp;**安装完成后记得删除下载的压缩包和不用的文件，否则打包成镜像的时候镜像会占用很大的空间。**



## 6、安装`Miniconda`
&nbsp; &nbsp; &nbsp; &nbsp;基本的深度学习环境配置好了，还需要安装python用于开发啊，为创建虚拟环境方便，我选用了`Miniconda`，体积小，还可以很方便配置开发环境。
&nbsp; &nbsp; &nbsp; &nbsp;安装的版本看个人需求，我用的`python3.8`的版本。官网下载速度太慢，我是从清华大学镜像源下载的，下载地址为：[Miniconda](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)，或在服务器中用命令下载：

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py38_4.8.3-Linux-x86_64.sh
```
如下图所示，下载完成后，先赋予可执行权限，然后执行安装即可，提示要输入的，输入`yes`即可，其他的一路`Enter`按下去。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185604.png)

安装完成后，重开一个终端，就是一下界面，说明安装完成：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185634.png)

**安装完成后记得删除`Miniconda`的安装包，否则打包会增加镜像占用空间。**

## 7、换源
&nbsp; &nbsp; &nbsp; &nbsp;有三个源可以换，`conda`源、`pip`源和`yum`源，镜像自带的`yum`源速度还可以，我就没换，只换了`conda`源和`pip`，换源就是为了是速度更快。这里都是使用清华的源，需要换其他源的大同小异，百度即可。
### （1）换conda源
创建`.condarc`文件：
```bash
vi ~/.condarc
```
将以下内容写入：

```bash
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```
清空缓存：

```bash
conda clean -i
```



### （2）换pip源

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```



## 7、打包容器为镜像

```bash
docker commit -a "wxj" -m "Server of DL" DL  dl_server:cuda10.2 
```

>  - `-a` 后跟作者
>  - `-m` 后跟镜像描述
>  - 此处的`DL`为我的容器名称，按自己的填写
>  -  `dl_server:cuda10.2` 这是我打包后的镜像名称和标签，按自己需求填写

 执行命令后等待一会，就可以看到打包后的镜像了
 ![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185655.png)



## 8、从镜像创建容器

和之前创建容器方式一样，映射好端口号就可以

```bash
nvidia-docker run -itd --name=dl_server -p 2223:22 --privileged=true --gpus all dl_server:cuda10.2  /usr/sbin/init
```
&nbsp; &nbsp; &nbsp; &nbsp;打开服务器对应防火墙后，就可以远程连接服务器跑模型了，也可以使用`PyCharm`专业版利用`SSH`连接进行远程开发，非常的舒服。