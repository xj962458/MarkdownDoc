## 一、介绍

​         Docker可以用于搭建很多有趣的项目，如各种系统、个人网盘、各种网站等，此处共介绍五个项目，都是通过网页进行访问，也是`Docker`应用最广泛的场景。通过实际操作这些项目，一方面可以巩固已学到的各种`Docker`知识，另一方面也可以培养对`Docker`的兴趣，从而对之后项目的开发具有促进作用。

搭建的五个项目如下，之后会一一介绍其安装访问过程，通过实际操作，将会更加巩固之前所学的理论知识。

* 基本系统
* 可道云个人网盘
* WordPress个人博客
* `VSCode`网页版`CodeServer`

* frp内网穿透服务

  

## xxxxxxxxxx docker-compose build 文件名bash

​        这里所说的基本系统，是利用`Docker`创建最基本的`Linux`系统，创建之后，可以当作普通的开发环境，利用`IDE`或`ssh`工具连接作为开发环境，也可以安装`nginx`、`apache`等服务器软件充当服务器使用。基本系统是`docker`容器最基本的东西，其他的各种服务，如个人云盘、个人博客和个人网站等，都是在其基础上安装相应的软件而实现的扩展。

​       本项目将使用选用`centos7`作为底层系统，安装`ssh`服务和宝塔面板，以实现开发环境和服务器的双重目的。

### 1、创建容器

```bash
docker run -d \
--name="MyCentOS" \
--privileged=true \
-p 2222:22 \
-p 8888:8888 \
centos:7 \
/sbin/init
```

参数解释：

* `-d`：后台运行

* `--name`：指定容器名称

* `--privileged=true`：开启特权模式，以能够正常使用`systemctl`

* `-p`：指定端口，格式为`宿主机端口:容器端口`

  * 第一处：容器端口为22，宿主机端口为2222，这是开放了`ssh`服务的端口。`ssh`服务的端口是22，现在将其映射到宿主机的2222端口，那么在宿主机上就可以通过2222端口访问容器系统的终端，从而实现各种操作。
  * 第二处：容器端口为8888，宿主机端口为8888，这是开发了宝塔面板的端口。宝塔面板的端口为8888，现在将容器的8888端口映射为宿主机的8888端口，那么就可以在容器外通过`ip地址`+8888端口号访问宝塔面板。
  * 除此之外，若需要其他端口，只需要多加几个`-p`参数即可，如若使用`MySQL`数据库，则可以加上`-p 3306:3306`，将容器的3306端口映射到宿主机的3306端口。
  * 若创建容器时指定端口不够，后续也可以修改和添加端口，将在下一篇中介绍。

* `centos:7`：此处指定了创建容器的镜像，为centos7，之所以不用最新版，是因为最新版已停止支持。

* `/usr/sbin/init`：这是使用特权模式所需要的，必须为这个

  

### 2、进入容器

```bash
docker exec -it MyCentOS bash
```

* `-it`：打开交互终端

* `MyCentOS`：刚才创建的容器名称

* `bash`：指定终端类型 

  

### 3、安装软件

​		需要安装的软件有两个：`openssh-server`和`wget`，`openssh-server`为`ssh`的服务端，而`wget`是一个下载工具，用于之后安装宝塔面板。

```bash
yum update -y && \
yum install openssh-server wget -y
```



### 4、设置ssh服务

​		在CentOS7上成功运行，但不知为何在`Ubuntu22.04`上运行出现问题，在`Ubuntu22.04`上以`Centos7`镜像创建的容器无法使用`systemctl`，从而无法控制`ssh`服务的启动。若在容器内运行`systemctl`命令会出现以下报错，可跳过`ssh`服务的安装问题。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711153742.png)

若未出现上述情况，可继续以下的执行：

① 启动ssh服务并设置开机自启

```bash
systemctl start sshd  # 开启ssh服务
systemctl enable sshd # 设置ssh服务开机自启
```

② 设置密码

```bash
passwd
```

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711153958.png)

设置完密码后可输入`exit`命令退出容器，返回宿主机终端

③ 连接

在宿主机终端中输入以下命令可连接容器终端：

```bash
ssh root@localhost -p 2222
```

也可通过宿主机的`ip`进行连接。除了使用终端连接，还可使用`ssh`工具或`PyCharm`等IDE进行连接，此处不多介绍。



### 5、安装宝塔面板

宝塔面板是一款可视化的服务器管理软件，对新手部署web应用较为友好。

宝塔面板官网：[https://www.bt.cn](https://www.bt.cn/)

进入容器终端，然后使用如下命令，进行宝塔面板的安装，脚本会自动进行安装，安装完成后

```bash
wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
```

安装完成后的界面如下，会显示访问地址和账号密码，即可进行访问，在宝塔面板中可以很方便的部署安装各种环境，也可以很轻松地部署网站。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711160348.png)
如上图，安装完成后会显示内外网的访问地址，一般用内网地址访问，外网访问一般无效。我这上面内网地址有些问题，需要修改以下:

```bash
http://:8888/8ba7b9b5 # 原地址
http://:localhost:8888/8ba7b9b5 # 虚拟机内部浏览器访问
http://:你的IP:8888/8ba7b9b5 # 外部浏览器访问
```


宝塔面板的界面如下图所示：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711155111.png)



### 6、总结

​       经过上述的安装，我们创建的容器就拥有了`ssh`功能和可视化部署环境和网页的功能。其实部署其他软件也和这个步骤类似：创建容器->安装软件->开放端口->封装成镜像。接下来介绍的几个项目，均是他人构建完成后的镜像，我们只需要根据镜像创建相应的容器，即可实现各种功能。



## 三、可道云个人网盘

### 1、介绍

​        可道云是一款网盘软件，用户界面非常友好，如Windows体验的私有云盘/企业网盘，完全支持私有化部署，存储安全可控，数百种文件格式在线预览、编辑和播放，轻松分享，高效协作，细粒度权限管控，全平台客户端覆盖，随时随地访问，轻松同步挂载。

官网地址：[http://www.kodcloud.com](http://www.kodcloud.com/)



### 2、创建容器

先创建一个存储数据的文件夹：

```bash
mkdir /data
```

然后再创建容器：

```bash
docker run -d \
-p 80:80 \
-v /data:/var/www/html \
kodcloud/kodexplorer
```

* `-d`：后台运行
* `-p 80:80`：将容器的80端口映射到宿主机的80端口，若端口被占用，可更换宿主机的其他端口
* `-v`：设置卷映射，将宿主机的`/data`目录映射为容器的`/var/www/html`目录，文件将同步修改
* `kodcloud/kodexplorer`：可道云的容器



### 3、访问

访问地址：

```bash
http://你的IP
或
http://你的IP:80
http://localhost # 虚拟机内部浏览器f
```

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711165958.png)

首次登录需要设置管理员密码，然后就可以登录了

登录后的界面如下，可以进行文件的共享和其他操作：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711170214.png)



## 四、WordPress个人博客

### 1、介绍

​	   `WordPress`是一个开源的个人博客，可以为个人更为快速地搭建自己的博客，且支持自定义。正常创建`WordPress`个人博客比较麻烦，需要安装`php`、`nginx`等各种软件，然后还要部署源码。但是使用`Docker`创建将会非常简单。

​      `WordPress`官网：[https://wordpress.com/zh-cn](https://wordpress.com/zh-cn/)

​       官方的`WordPress`镜像不会自己创建数据库，所以需要指定数据库或创建一个数据库容器使其互联。使用`Docker`创建`WordPress`有很多方法，可以自己一步步创建容器，再将容器进行互联，也可以使用`Docker-compose`进行创建，此处只介绍第一种方法。



### 2、创建容器

#### （1）创建MySQL容器

```bash
docker run -d \
--restart=always \
--name="WordPressMySQL" \
-e MYSQL_ROOT_PASSWORD=12345678 \
-e MYSQL_DATABASE=wordpress \
-v mysql:/var/lib/mysql \
mysql:latest
```

* `-d`：后台运行

* `--restart`：设置重启策略，设为always为容器若停止则总是重启；

* `--name`：设置容器名称；

* `-e`：设置环境变量：

  * `MYSQL_ROOT_PASSWORD`：`root`账户的密码;
  * `MYSQL_DATABASE`：要创建的数据库名称，此处创建用于之后wordpress连接；

* `-v`：设置卷映射；

* `mysql:latest`：创建容器所有镜像。

  

#### （2）创建wordpress容器

```bash
docker run -d \
--restart=always \
--name="WordPress" \
-p 9080:80 \
-e WORDPRESS_DB_HOST=mysql \
-e WORDPRESS_DB_USER=root \
-e WORDPRESS_DB_PASSWORD=12345678 \
-e WORDPRESS_DB_NAME=wordpress \
-v wordpress:/var/www/html \
--link WordPressMySQL:mysql \
wordpress:latest
```

* `-d`：后台运行；
* `--restart`：设置重启策略，设为always为容器若停止则总是重启；
* `--name`：设置容器名称；
* `-e`：设置环境变量;
  * `WORDPRESS_DB_HOST`：数据库地址，为mysql即可，也可填ip地址；
  * `WORDPRESS_DB_USER`：数据库用户名，刚才创建的；
  * `WORDPRESS_DB_PASSWORD`：数据库密码，刚才创建的；
  * `WORDPRESS_DB_NAME`：要连接的数据库名称，即为刚才创建的wordpress；
* `-v`：设置卷映射；
* `--link`：将此容器与刚才创建的`MySQL`容器互连，处于一个子网中；
* `wordpress:latest`：创建容器所有镜像。



### 3、访问

刚才设置的访问端口是9080端口，那么我们可以在浏览器中输入以下地址进行访问：

```bash
http://IP:9080    # 虚拟机内外部均可访问
http://localhost:9080 # 虚拟机内部访问
```

访问后的界面如下，需要初始化：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712103509.png)

设置好语言后，需要设置网站标题、用户名、密码和邮箱等，然后点击`安装WordPress`
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712103639.png)

登录后的界面如图所示，可以发表和管理博客，也可以对界面进行管理，还可以安装插件增加更多功能等。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712103752.png)



## 五、`VSCode`网页版`CodeServer`

### 1、介绍

​		`CodeServer`是`VSCode`的网页版本，只要搭建完成，在电脑、手机和平板等各种浏览器中都可以访问，访问后可以实现编写代码并运行的效果，非常适合移动编码和轻量编码的人群。除了`VSCode`有网页版，可以在`Docker`容器中创建，`JetBrain`系的软件也可以通过`Docker`创建，然后在网页中访问，该项目名称为`JetBrains Projector`，其相关信息请查阅：[https://lp.jetbrains.com/projector](https://lp.jetbrains.com/projector/)



### 2、创建容器

先创建个文件夹用于存储配置文件：

```bash
mkdir -p ~/.config
```

再创建容器：

```bash
docker run -d \
--name code-server \
-p 8080:8080 \
-v "$HOME/.config:/home/coder/.config" \
-v "$PWD:/home/coder/project" \
-u "$(id -u):$(id -g)" \
-e "DOCKER_USER=$USER" \
codercom/code-server:latest
```

* `-d`：后台运行

* `--name`：指定容器名称

* `-p`：设置端口映射，将容器的8080端口映射到宿主机的8080端口

* `-v`：卷映射，就是目录映射，将宿主机的目录映射到容器的目录，以实现同步更新和数据的安全

* `-u`：指定用户名或UID

* `-e`：指定环境变量的值

* `codercom/code-server:latest`：创建该容器的镜像

  

### 3、查看或修改密码

首先进入容器：

```bash
docker exec -it code-server bash
```

查看配置文件：

```bash
cat ~/.config/code-server/config.yaml
```

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711194613.png)
如上图所示，password后就是密码

如果要修改密码，请执行以下命令:

```bash
nano ~/.config/code-server/config.yaml
```

将`password`修改为你想要的密码之后，输入`exit`退出容器，然后重启容器后生效

重启容器命令如下：

```bash
docker restart code-server
```



### 4、访问

记住刚才查看或修改的密码，然后通过以下地址访问：

```bash
http://你的IP:8080
http://localhost:8080
```

页面如图所示：
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711195457.png)

​        输入密码后点击`SUBMIT`后即可登录，登录后的页面如下图所示，和标准的`VSCode`没什么区别，`VSCode`能够实现的功能在这上面都可以实现，可以按照插件实现汉化和各种语言支持。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711195649.png)



## 六、frp内网穿透服务

### 1、介绍

​	    frp是一个内网穿透服务，包含服务端frps和客户端frpc。内网穿透，通俗来讲，就是在互联网上访问没有公网ip的内网服务，比如，我自己在家里面的电脑上搭建了一个网站，我想让全世界的人都可以访问这个网站，那么我就需要配置内网穿透服务。搭建frp服务需要一台拥有公网ip的电脑，最好是Linux系统，然后在上面按照frps服务，但是frps服务会占用掉电脑的80、443等常用端口，如果一台电脑或服务器只用个frp服务就太浪费了，所以这里介绍一种使用docker搭建该服务的方法。

​		frp是一个开源项目，其开源地址为：[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

​		以下介绍搭建frpc和frps两种服务，frps最好在服务器上进行搭建，而frpc可以在需要用到内网穿透服务的电脑上进行搭建。

​		此处使用第三方镜像，所使用的frps和frpc可能不是较新版本，但功能正常，也可以自己使用最新版本的frpc和frps封装镜像，但较为麻烦，此处不多介绍。使用的`docker`镜像及其按照教程来源于`github`开源项目，项目地址为：[https://github.com/clangcn/frp-docker](https://github.com/clangcn/frp-docker/)





### 2、创建frps容器

#### （1）说明

​     `frps`是`frp`内网穿透服务的服务端，它也可以以插件的形式安装，也可以以软件的形式安装，在`openwrt`、`padavan`等路由系统中常常集成，此处介绍在`Docker`容器中安装，以不影响服务器上的其他服务。一个`frps`服务端可以对应多个`frpc`客户端，理论上来说，服务器有多少个可用端口，就可以对应多少个`frpc`客户端，所以即使多个设备使用内网穿透服务，`frps`客户端也只需安装一个即可。



#### （2）创建容器

使用的镜像未推送到DockerHub，所以需要先下载导入后再创建容器

##### ① 下载镜像

```bash
wget --no-check-certificate https://code.aliyun.com/clangcn/frp-docker/raw/master/frps-docker/frps-docker.tar
```

##### ② 导入镜像

```bash
docker load < frps-docker.tar
```

##### ③ 创建容器

```bash
docker run -h="frps-docker" --name frps-docker -d \
-p 6443:5443/tcp \
-p 6443:5443/udp \
-p 6444:5444/udp \
-p 7443:5445/tcp \
-p 8080:80/tcp \
-p 8443:443/tcp \
-e set_token=password \
-e set_subdomain_host= \
-e set_max_pool_count=50 \
-e set_log_level=info \
-e set_log_max_days=3 \
"frps-docker:latest"
```

##### ④ 端口说明

| Docker内定义     | Docker内默认值 | 描述                    |
| :--------------- | :------------: | :---------------------- |
| bind_port        |   5443(TCP)    | frps服务端口            |
| kcp_bind_port    |   5443(UDP)    | KCP加速端口             |
| bind_udp_port    |   5444(UDP)    | udp端口帮助udp洞洞穿nat |
| dashboard_port   |   5445(TCP)    | Frps控制台端口          |
| vhost_http_port  |    80(TCP)     | http穿透的端口。        |
| vhost_https_port |    443(TCP)    | https穿透服务的端口     |

若需要其他端口，可以自己添加，如需要3306端口和22端口，可以将添加如下参数，`\`为续行符：

```bash
-p 3306:3306 \
-p 2222:22 \
```



##### ⑤ 变量说明

##### 变量名区分大小写

| 变量名                     |  默认值  | 描述                                                    |
| :------------------------- | :------: | :------------------------------------------------------ |
| set_token                  | password | frps的认证密码，用于客户端连接                          |
| set_subdomain_host         |          | frps子域名设置，默认为空，可以输入类似abc.com这样的域名 |
| set_dashboard_user         |  admin   | frps控制台用户名                                        |
| set_dashboard_pwd          |  admin   | frps控制台密码                                          |
| set_max_pool_count         |    50    | 最大连接池数，貌似不用这个了                            |
| set_max_ports_per_client   |    0     | 允许连入的最大客户端，0为不限制                         |
| set_authentication_timeout |   900    | 验证时间，单位为秒，默认900s                            |
| set_log_level              |   info   | 日志等级，可选项：debug, info, warn, error              |
| set_log_max_days           |    3     | 日志保存天数，默认保存3天的                             |
| set_tcp_mux                |   true   | TCP 多路复用                                            |

​        以上参数为容器系统的环境变量，不设置会保持默认，如若不设置账号密码，则账号密码均为`admin`。

需要注意的是，此处的`token`值要牢记，frpc客户端使用frps服务端时，需要该值。



##### ⑥ 其他说明

* **容器创建的端口，若想在外网访问，需要打开服务器的防火墙，运行服务端口通过。**

  

#### （3）访问frp管理面板

访问地址为：

```bash
http://服务器IP:7443
```

![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220711215516.png)
此处可以查看使用frp服务的端口和流量使用情况。



* **创建完frps服务的容器之后，需要记住`服务器的IP`、`端口号5443`、`需要用到的端口`和创建容器时设置的`token`，若客户端使用该服务端，需要用到以上所说的几项。**

* 当`frpc`客户端连接上`frps`服务时，在`Proxies`选项中可以看到连接的信息，如下图所示，此`frps`服务端连接了三个服务，在这个面板中可以查看它们的在线情况、使用端口和流量情况等信息。
  ![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220712092110.png)
  



### 3、创建frpc容器

#### （1）说明

​         `frpc`是`frp`内网穿透服务的客户端，它也可以以插件的服务安装，也可以以软件的形式安装，在`openwrt`、`padavan`等路由系统中常常集成，也就是说只要路由器上拥有`frp`服务，那么就可以让路由器下的所有设备使用内网穿透服务，从而实现在互联网上访问局域网中的服务。此外，对于一个没有公网IP的服务器，若想在互联网上访问其上的服务，最简单的方法就是在服务器中安装一个`Docker`，然后在其中创建一个`frpc`容器，那么服务器的所有服务都可以使用内网穿透服务。



#### （2）创建容器

① 下载镜像

```bash
wget --no-check-certificate https://code.aliyun.com/clangcn/frp-docker/raw/master/frpc-docker/frpc-docker.tar
```



② 导入镜像

```bash
docker load < frpc-docker.tar
```



③ 创建配置文件

（i）首先下载配置文件模板

```bash
wget --no-check-certificate https://code.aliyun.com/clangcn/frp-docker/raw/master/frpc-docker/frpc.ini -O ~/frpc.ini
```

（ii）对其进行修改：

```bash
vim ~/frpc
```

（iii）若无`vim`可安装：

```bash
apt install vim -y # Debain、Ubuntu和Deepin等Debain系Linux
yum install vim -y # CentOS、RedHat等RedHat系Linux
```

（iv）`frpc.ini`文件内容如下：

```ini
# [common] is integral section
[common]
# A literal address or host name for IPv6 must be enclosed
# in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80"
server_addr = your server ip
server_port = 5443

# for authentication
token = password

# connections will be established in advance, default value is zero
pool_count = 5

# if tcp stream multiplexing is used, default is true, it must be same with frps
tcp_mux = true

# your proxy name will be changed to {user}.{proxy}
user = admin

# decide if exit program when first login failed, otherwise continuous relogin to frps
# default is true
login_fail_exit = false

# console or real logFile path like ./frpc.log
log_file = ./frpc.log
# trace, debug, info, warn, error
log_level = info
log_max_days = 3

# communication protocol used to connect to server
# now it supports tcp and kcp, default is tcp
protocol = tcp

# Resolve your domain names to [server_addr] so you can use http://web01.yourdomain.com to browse web01 and http://web02.yourdomain.com to browse web02
[test]
type = http
local_ip = 192.168.1.1
local_port = 80
use_encryption = true
use_compression = true
custom_domains = test.domain.com
```

（v）需要修改的地方如下

```ini
server_addr = your server ip  # 修改为搭建frps服务的服务器ip地址 
server_port = 5443            # 按本教程的frps搭建不用修改该端口
token = password              # 创建frps服务时设置的token
user = admin                  # 这个可改可不改，就是一个标识
```

（vi）然后添加的服务的配置

```ini
[test]                            # 服务名称，标识以下你的内网穿透服务
type = http                       # 网络协议，用到80端口写http，用到443端口写https，用到其他端口写tcp即可
local_ip = 192.168.1.1            # 使用内网穿透服务设备的局域网ip，如我电脑要使用，此处填电脑的ip
local_port = 80                   # 使用内网穿透服务的服务端口
use_encryption = true             # 不用动
use_compression = true            # 不用动
custom_domains = test.domain.com  # 设置二级域名用的，可选择将其用'#'注释掉
```

有多少个服务，就可以创建多少个以上的配置文件，如我现在电脑的局域网ip为`192.168.68.123`，然后我开了两个服务，一个是宝塔面板，在8888端口，一个是`jupyterlab`，在8080端口，我若想在公网上访问，可以添加如下配置：

```ini
[btpanel]
type = tcp
local_ip = 192.168.68.123
local_port = 8888

[jupyterlab]
type = tcp
local_ip = 192.168.68.123
local_port = 8080
```



④ 创建容器

配置文件创建好后就可以创建容器了，创建的容器使用之前设置的配置文件，创建容器的命令如下：

```bash
docker run 
-h="frpc-docker" \
--name frpc-docker \
-d \
-v ~/frpc.ini:/usr/local/frpc/frpc.ini \
"frpc-docker:latest"
```



###  4、总结

​       `frp`内网穿透服务方便了在互联网中访问内网服务，而`Docker`又简化了这一过程，`frp`服务是比较稳定的，我使用很长时间都没有出现过问题。只要配置正确，那么就可以很简单地实现内网穿透服务，在外就可以访问家中的服务器、NAS等设备，当然，访问的速度取决于公网服务器的带宽，frp内网穿透服务本质上其实只是做了一次中转。



