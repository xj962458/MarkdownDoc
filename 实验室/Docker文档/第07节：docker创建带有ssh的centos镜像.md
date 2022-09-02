# 一、介绍

​       一般`Docker`镜像是不带`ssh`服务的，但是我们操作`Linux`系统时，常使用`SSH`工具进行连接。所以需要创建带有`SSH`功能的容器，创建后可以通过`ssh`工具连接，从而实现远程操作的目的。在CentOS7上成功运行，但不知为何在`Ubuntu22.04`上运行出现问题。



# 二、操作
## 1、拉取镜像
```bash
docker pull centos:7
```

此处使用`Centos`7镜像作为基本的构建镜像，使用`Ubuntu`镜像作为基本镜像构建会出现一些权限问题。



## 2、生成容器

```bash
docker run -itd --name=centos_ssh --privileged=true centos:7 /usr/sbin/init
```
>1、其中--name指定容器名称
>2、--privileged=true 给容器访问Linux内核特权,后面要使用systemctl，不加--privileged=true后面将无法执行systemctl命令。



## 3、进入容器

```bash
docker exec -it centos_ssh bash
```



## 4、在容器内配置

### （1）安装passwd和ssh-server

```bash
yum update -y && yum install passwd -y &&yum install openssh-server -y
```
### （2）修改密码

```bash
passwd
```
之后会让你输入密码两次，记住密码即可

### （3）编辑配置文件
**该操作是为配置公钥连接做准备，若不需要公钥连接，可以忽略这一步**
请移步到[SSH公钥登录](https://blog.csdn.net/qq_29183811/article/details/121342732)

### （4）开启ssh并设置开机自启动

```bash
systemctl restart sshd
systemctl enable sshd
```



## 5、将容器打包成镜像

```bash
docker commit -m "centos with ssh" -a "root" centos_ssh centos_ssh:latest
```

> -m 来指定提交的说明信息
> -a 可以指定更新的用户信息 
> 第一个centos_ssh是容器名称 
> 第二个centos_ssh是要生成的镜像名称，latest是版本号，默认是`latest`

使用

```bash
docker images
```
就能够看到刚生成的镜像了，生成镜像后可以将刚才的用于创建镜像的容器停止并删除，以节约资源。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712185803.png)



## 6、通过生成的镜像创建容器

```bash
docker run -d -p 2222:22 centos_ssh /usr/sbin/sshd -D
```
> -p是指定端口号，格式为主机端口号：容器端口号，即将容器相应端口号映射到主机相应端口号



## 7、连接

### （1）密码连接
```bash
ssh root@localhost -p 2222
```
> 2222为你刚才指定的主机端口号

### （2）公钥连接
主机中使用命令

```bash
ssh-keygen -t rsa
```
生成公钥，然后将公钥复制后粘贴在容器中的`~/.ssh/authorized_keys`中，可以参考[SSH公钥登录](https://blog.csdn.net/qq_29183811/article/details/121342732)
配置完成后使用
```bash
ssh root@localhost -p 2222
```
即可连接，且不需要再输入密码



## 8、使用Dockerfile构建镜像

dockerfile内容如下：

```bash
#生成的新镜像以centos7镜像为基础
FROM centos:7
#升级系统
RUN yum -y update
#安装openssh-server
RUN yum -y install openssh-server
#修改/etc/ssh/sshd_config
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# 生成sshkey
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key

#变更root密码
RUN echo "root:你要修改成的密码"|chpasswd
#开放窗口的22端口
EXPOSE 22
#运行脚本，启动sshd服务
CMD    ["/usr/sbin/sshd", "-D"]
```
然后使用

```bash
docker build -t centos_ssh .
```
构建镜像即可。构建镜像后可以通过

```bash
docker images
```
查看构建好的镜像，然后就可以通过构建好的镜像创建容器并进行连接了。
