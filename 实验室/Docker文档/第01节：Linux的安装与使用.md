## 一、介绍
​	   Linux系统被广泛应用于服务器操作系统，其开源免费、高效、生态良好等特性深受各大行业的喜爱。Docker是基于Linux开发的，在Linux上运行更为高效，在Windows和MacOS上也可以安装，但远没有Linux上高效、利用管理。除此之外，Docker所创建的容器，多为Linux系统，所以要学习Docker，必须对Linux有一定的了解。第一节首先介绍并安装Linux系统，然后再在Linux系统上安装Docker，第二节中`Docker的介绍与安装`中也会提供Windows上的安装方法，但并不推荐这样使用。此处选用的Linux发行版为Ubuntu，版本为最新的Ubuntu22.04。
​      Linux系统有多种安装方法，如实体机安装，就是将系统的系统全部安装为Linux系统，双系统安装，即安装与本机系统共存的Linux系统，还有wsl、虚拟机安装等。本次具体介绍虚拟机安装，较为方便快捷，且对电脑原本的系统影响较小。



## 二、安装VMware Workstation

​      若想在Windows上使用Linux系统，其中一种最简便的方法便是使用虚拟机。虚拟机软件有很多，`Vmware Workstation`、`VirtualBox`和`HyperV`等，此处使用Vmware Workstation来演示。

### 1、下载

[Vmware Workstation 下载地址](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)

​        下拉界面，选择Windows版本的，点击立即下载即可。Vmware Workstation有很多个版本，建议下载最新的16版本，因为之前的版本与Windows自带的HyperV虚拟机不兼容，会导致无法运行。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705083350.png)



### 2、安装

下载后双击运行，不用修改设置，一路下一步即可，如果需要更改安装位置，可在此界面更改：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705090509.png)

在最后一步，如下图所示，先不要点击完成，点击许可证，用于激活：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705101802.png)



### 3、激活

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705101830.png)
如上图所示，在许可证地方输入激活密钥即可激活，可用激活密钥如下，任选一个即可，填入后点击输入完成激活。

```bash
ZF3R0-FHED2-M80TY-8QYGC-NPKYF
YF390-0HF8P-M81RQ-2DXQE-M2UT6
ZF71R-DMX85-08DQY-8YMNC-PPHV8
```



## 三、下载Linux系统

本次选用的Linux操作系统为Ubuntu22.04，可去其官网下载：[Ubuntu22.04下载地址](https://cn.ubuntu.com/download/desktop)
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705115122.png)
如上图所示，点击下载即可，也可以安装其他版本的Ubuntu系统，下载方式大同小异。

下载好的文件为`*.iso`文件，记住保存的位置，之后有用：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705120145.png)`


## 四、创建虚拟机
安装好`Vmware Workstation`，下载了`Linux`系统镜像后，就需要创建虚拟机了。

### 1、新建

打开`Vmware Workstation`，选择新建—>新建虚拟机
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220705115713.png)



### 2、选择类型

一般选择典型即可，也可选择自定义，自己一步步配置

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709155234.png)



### 3、选择安装的镜像

找到之前下载的`Ubuntu`系统镜像，选中
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709153827.png)
`Vmware Workstation`会自动检测出系统的镜像类型，简化之后的安装过程



### 4、配置系统

填写用户名和密码等信息，在安装时会`Vmware Workstation`会自动配置
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709154022.png)



### 5、硬盘配置

默认是20G，运行`Docker`可能会比较小，可多设置一些
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709154150.png)



### 6、其他配置

一些配置如果想自定义，可以点击自定义硬件进行修改，因为勾选了`创建后开启此虚拟机`，所以创建完成后会自动进行启动安装系统。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709154257.png)

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709154358.png)

配置完成后会自动安装，可不干预安装过程，也可干扰安装过程，进行自定义操作。



## 五、打开虚拟机

​       系统安装后会自动重启，重启后输入密码可进入系统。进入系统后可进行换源、安装输入法、安装语言支持和安装软件等一系列操作，但Docker主要使用终端进行操作，以上所说用的较少，就不多介绍，需要的可以自行了解。

### 1、全屏显示
如图，可点击查看->全屏，让虚拟机进行全屏显示，如果有分辨率问题，可在Linux系统中修改分辨率。

若需要在宿主机和虚拟机直接复制文件，可按照`vmware tools`，此处不多介绍。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709160624.png)



### 2、打开终端

之后的很多操作都是在终端中完成，在`Ubuntu`系统中，可使用快捷键`Ctrl+Alt+T`打开终端，在终端中可以输入命令，进行很多操作。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709164429.png)



### 3、ssh登录

​        在虚拟机里面输入命令，若不安装`vmware tools`，则无法直接从`Windows`复制粘贴文件到`Linux`终端执行，配置好`ssh`命令后，就可以直接通过`ssh`工具直接连接虚拟机中的终端。`ssh`连接工具有很多，如`XShell`、`Putty`和`Termius`，此处为简单，不安装其他工具，直接利用`Windows`自带的`ssh`工具进行连接，感兴趣的可以自行探索其他工具。

#### （1）安装软件

先在虚拟机的终端中执行以下命令：

```bash
sudo apt install openssh-server net-tools -y
```
安装了两个工具，一个是`ssh`服务端，一个是网络工具，用于查看IP地址
设置`ssh`启动和开机自启
```bash
systemctl start sshd
systemctl enable sshd
```



#### （2）查看网络信息

​       安装完成后在终端中输入`ifconfig`，得到的结果如下图所示，箭头所指的ip地址即为虚拟机的ip地址，每个人都不一定相同，以自己的为准。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710100850.png)



#### （3）打开`Windows`终端

​        在Windows中按`Win`+`R`，输入cmd，打开终端，不同系统终端样式不同，但打开方式一样。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712182347.png)



#### （4）连接
在打开的`Windows`终端中输入以下命令

```bash
ssh 用户名@IP
```
用户名即为虚拟机设置的用户名，IP为刚才查看的IP。之后输入`yes`，然后再输入设置的虚拟机密码即可连接虚拟机内的终端。



![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712183336.png)



#### （5）其他

除此之外，还可以安装语言支持、更换软件源，安装软件等操作，此处不多介绍。



## 六、Linux常用命令

以下命令，均在终端中输入，这些都是些最基本的指令，在之后的Docker学习中也会用到

### 1、列出文件和目录名称
* ls列出文件基本文件和名录（list）
* ls -la 列出详细信息及其隐藏文件
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709164732.png)



### 2、切换目录

* cd 目录名称：切换目录（change directory）
* pwd：打印当前文件路径(print work directory)
* ~ ：指代当前用户家目录
* /：系统根目录
* ..：上级目录
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709165037.png)



### 3、创建目录

mkdir 目录名称：创建新目录（make directory）
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709165950.png)

若创建多级目录，需要添加`-p`参数，如：

```bash
mkdir -p /root/.ssh
```



### 4、复制文件

cp 原文件 新文件：从源文件复制，得到新文件（copy）
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709165804.png)



### 5、移动文件或目录

mv 原文件路径 新文件路径：移动文件或目录（move）
该命令还可作为重命名命令使用
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709170211.png)



### 6、删除空目录

rmdir 目录名：删除空目录（remove directory），删除非空目录会报错

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710092832.png)



### 7、删除文件或目录

删除空目录，可以使用`rmdir`命令，而删除非空目录和文件需要使用`rm`命令
```bash
rm [参数] 文件/目录
```
参数：
* -f ：强制删除
* -r：递归删除
如果不加`-r`参数是无法删除目录的，只能够删除文件。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710093256.png)

​        **`rm`命令虽然很强大，且和`-r`、`-f`参数搭配之后更为强大，但是删除后的文件无法恢复，所以请慎用。`Linux`系统的特殊机制，`root`用户可以删除系统，使用以下命令会导致系统所有文件被删除，无法恢复，切勿在生产环境中使用。**

```bash
rm -rf /*
```



### 8、文件编辑

在命令行中对文件进行编辑，可使用`nano`或`vi/vim`命令

① `nano`
`nano` 文件名：即可编辑文件，若文件存在，则直接编辑，不存在则进行创建
`nano`界面如图所示，可输入内容，编辑完成后按`Ctrl+X`保存文件
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709170651.png)

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709170804.png)

② `vi/vim`
       `vi`或`vim`也是文本编辑器，用法类似，使用`vim`可能需要安装，所以可先使用`vi`用法如下：

`vi/vim`文件名：即可编辑文件，若文件存在，则直接编辑，不存在则进行创建

`vim`打开文件后，需要按`a`或`i`才能够开始编辑，保存则需要先按`Esc`键后再输入`:wq`进行文件保存
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709171339.png)



### 9、文件权限管理

`chmod`命令可以对文件的权限进行管理，常使用的操作如下:

```bash
chmod +x 文件 # 赋予文件可执行权限
chmod +w 文件 # 赋予文件可写权限
chmod +r 文件 # 赋予文件可读权限
chmod +rwx 文件 # 赋予文件可读、可写和可执行权限
```

​        最常用的操作是赋予文件可写权限和可执行权限，如某文件需要编辑，但无法写入，需要赋予其可写权限，刚下载或编译的可执行文件无运行权限，需要服务可执行权限才可运行。

`chmod`命令十分强大，但使用时也要小心，不要随意修改系统文件权限，否则会导致系统崩溃。



### 10、用户切换

​       `Linux`是多用户多任务操作系统，允许多个用户同时登录，其中最高权限用户为`root`，可以对其他用户进行授权、增加和删除等操作。使用`Docker`一般需要用的`root`用户，所以此处介绍如何切换用户，切换用户使用`su+用户名进行切换`。
切换`root`用户有两种操作，`sudo su`或`su root`
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220709171830.png)



### 11、Ubuntu软件包管理

​        在`Windows`系统中，安装软件，我们需要从软件商店或从官网下载安装包后进行安装，`Linux`也可以使用这种安装方法。但是在常见的Linux系统中，还存在包管理工具，利用这些工具，只需要一条命令，就可以安装软件。

​       在`Ubuntu`这个Linux发行版中，有多个软件包管理工具，如`apt`、`apt-get`、`snap`等，用户也可以自行安装其他包管理工具，此处重点介绍`Ubuntu`系统中的`apt`命令，`apt`和`apt-get`用法基本相同，可以混用。

​        **安装软件时，需要在`root`用户下执行，若在普通用户下执行，则需要加`sudo`前缀，以下命令默认在`root`用户下执行，所以省略了`sudo`，若是在非`root`用户条件下执行，需加`sudo`，不然执行报错。**

#### ① apt软件包管理工具

在终端中执行`apt`命令，就会显示`apt`的各种用法，其他软件包管理工具同理。
<img src="http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710095725.png" style="zoom:50%;" />



#### ② 更新软件源

`Linux`在安装软件之前，都需要更新以下软件源，这样才能够确保包管理工具能够正确查找到软件，更新软件源的命令为:

```bash
apt update -y
```



#### ③ 安装软件

使用`apt`安装软件很简单，不过需要知道软件包的名称，然后使用以下命令进行安装

```bash
apt install 软件包 -y
```

常用的软件安装命令如下:

```bash
apt install openssh-server -y # 安装ssh服务端
apt install net-tools -y      # 安装网络管理工具，安装后才可使用ifconfig命令查看网络信息
apt install vim -y            # 安装vim文本编辑工具
apt install neofetch -y       # 安装neofetch，可用于显示系统配置
apt install gcc g++ -y        # 安装C语言和C++语言的编译器
```



#### ④ 移除软件

```bash
apt remove 软件包名称 -y
```



#### ⑤ 清理不用的包

移除软件后可能有很多不需要的文件未删除，此时可以使用以下命令彻底清除干净

```bash
apt autoremove -y
```



### 12、网络信息查看

```bash
ifconfig
```

此命令用于查看网络信息，对之后的使用有很大的帮助，若系统没有安装，可使用以下命令安装

```bash
apt install net-tools -y
```

​        使用`ifconfig`命令查看网络信息如下，如图所示，一般第二个或第一个为本机的网络信息，之后使用`Docker`搭建网络服务时，在局域网中访问需要知道本机IP

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220710100850.png)

除此之外，还有很多有用的Linux命令，在此处不多介绍，感兴趣的可以自行查阅
