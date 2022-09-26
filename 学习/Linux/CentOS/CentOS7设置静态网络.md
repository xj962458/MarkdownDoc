## 一、描述

`CentOS7`默认是动态获取IP地址（DHCP），平时使用没什么问题，但是当搭建一些网站，或对外提供服务时，不想IP改变，这就希望更改为静态IP地址。



## 二、修改

### 1、查看本机网络信息

```bash
ifconfig
```

若提示没有命令，先安装：

```bash
yum install net-tools -y
```

获取的网络信息如下，不同电脑所显示的有所差异，`docker0`、`eth0`和`lo`均可认为是网卡，有的是虚拟的，有的是实体的。我们要找实体的网卡，查看地址，此处`eth0`为实际网卡，常见的实际网卡有`eth0`、`eth1`、`eth2`和`em1`、`ens33`等，查看网卡中`inet`的信息。

<img src="http://doc.xjfyt.top/markdown_img/20220926202532.png" style="zoom: 50%;" />



### 2、找到网卡配置文件

```bash
cd /etc/sysconfig/network-scripts/
ls
```

![image-20220926202434530](http://doc.xjfyt.top/markdown_img/20220926202435.png)

此文件夹下有很多的网卡配置文件，第一步我的网卡是`eth0`，所以在此我要修改`ifcfg-eth0`文件，若你的网卡是`ens33`，则要修改`ifcfg-ens33`



## 3、修改配置文件

修改文件前记住第一步获取的`inet`中的IP地址，要修改的IP地址要和之前的网络在同一网段，且不能冲突，所以建议就修改为刚才查看到的IP地址。我第一步获取的IP地址为`192.168.123.92`

```bash
vim ifcfg-eth0
```

```bash
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"    # 此处dhcp改为static
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="eth0"
UUID="44f6f506-b395-4578-b51b-2225a6ed1484"
DEVICE="eth0"
ONBOOT="yes"

# 以下为添加内容
DNS1=8.8.8.8              # DNS服务器一，可如此填写
DNS2=114.114.114.114      # DNS服务器二，可如此填写
IPADDR=192.168.123.92     # 要设置的静态IP地址，此处可设置为你的
NETMASK=255.255.255.0     # 子网掩码，默认即可
GATEWAY=192.168.123.1     # 网关地址
```

注意，网关地址和你的路由器有关，一般填写路由器对应的IP地址即可，`192.168.*.*`的IP更改为`192.168.*.1`即可，若是在`VMware`虚拟机中，网关一般为`192.168.*.2`



### 4、重启网络

修改后需要重启网络或重启系统，重启网络的命令如下：

```bash
service network restart
```

