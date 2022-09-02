## 一、可视化面板介绍
​       之前学习的Docker都是在终端中输入命令来执行Docker的操作，虽然更为高效快捷，但显示信息不够直观，对新手不太友好，接下来介绍可视化管理Docker的方法——Docker面板。使用面板的好处是，不用记复杂的命令，并且可视化，管理方便。Docker可视化管理面板有很多，如`Portainer`、FAST OS Docker和Docker Desktop等，多数是在网页中进行操作的，此处介绍前两种面板的安装方法。

## 二、搭建Portainer-CE
​       **官方地址：**[https://www.portainer.io](https://www.portainer.io/)
​       `Portainer`是一个广泛为人所知的一个`Docker`面板，使用人数较多。`Portainer-CE` 是`Portainer`的社区版本，除此之外，还有商业版本的，社区版本是免费的，功能基本够用，不过该面板官方没有中文版本，如果需要中文，需要安装汉化文件或使用第三方镜像。此处介绍官方版和汉化版安装方法，汉化版使用第三方镜像，汉化版和官方版安装一种即可，若想都安装，需要自行修改端口。

### 1、官方版本（英文版）
#### ①  安装命令
终端执行命令：
```bash
docker run \
-d \
-p 8000:8000 \
-p 9443:9443 \
--name="portainer" \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \ 
-v portainer_data:/data \
portainer/portainer-ce:latest
```



#### ②  访问

命令会自行拉取镜像，并通过镜像构建容器，完成后打开浏览器，输入以下内容：
```bash
https://ip:9443
```
ip为安装docker机器的ip，可以通过`ifconfig`命令查看，若是在虚拟机内部进行访问，可以打开`Ubuntu`自带的`FireFox`浏览器，输入以下网址进行访问：
```bash
https://localhost:9443
```
若出现以下界面，无视即可：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711112245.png)



#### ③ 设置密码

访问后，会看到如下界面，设置账号密码，点击`Create User`即可创建用户，创建用户后会跳转到登录界面
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711094600.png)



#### ④ 登录

输入之前设置的账号和密码即可登录
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711112835.png)



#### ⑤ 主界面

登录后进入主界面，在这里可以对面板和docker进行管理，因为是图形化界面，较为简单，此处不多介绍。接下来介绍如何安装中文版
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711112922.png)



### 2、第三方版本（汉化版）

#### ①  安装命令
如果已经安装了英文版，需要先删除，或者更改容器的对外端口，否则会造成端口冲突而创建失败。
```bash
docker run \
-d \
--restart=always \
--name="portainer" \
-p 8000:8000 \
-p 9443:9443 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
6053537/portainer-ce
```



#### ②  主界面

​       安装完成后，访问方式和英文版本访问方式一样，此处不多介绍，中文版界面如图所示。中文版就是在官方版本的基础上加上了汉化文件罢了，也可以在官方版本上直接添加汉化文件进行汉化，此处不多做介绍。

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711113319.png)




## 三、搭建FAST OS DOCKER面板

​        **官方地址：**[https://www.dockernb.com](https://www.dockernb.com/)

​        这款是国产中文面板，界面简洁，功能也不错，不过尚在完善开发中，但目前的功能基本够用，是中文版本的，对英文不好的人比较友好。

### 1、安装命令

```bash
docker run \
-d \
--restart always \
-p 8081:8081 \
-e TZ="Asia/Shanghai" \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /etc/docker/:/etc/docker/ \
wangbinxingkong/fast:latest
```



### 2、访问

访问地址为：

```bash
http://ip:8081
```

`ip`即为安装docker的`ip`地址，在虚拟机内部浏览器访问可用以下网址访问：

```bash
http://localhost:8081
```



### 3、注册
访问后进入如下界面，点击注册，去注册账号
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711122039.png)



注册界面如下：
<img src="http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711122145.png" style="zoom: 80%;" />
注册完成后会自动登录



### 3、主界面
主界面如下，比`Portainer`更为简洁，且原生为中文，使用方法也不多介绍，和`Portainer`大同小异。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711122225.png)