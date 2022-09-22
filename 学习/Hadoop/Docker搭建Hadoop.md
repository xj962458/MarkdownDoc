# Docker安装Hadoop



## 一、创建容器

先创建三台容器：

* 三台容器分别命名为hadoop01、hadoop02和hadoop03
* 将三台容器的主机名（hostname）分别设置为hadoop01、hadoop02和hadoop03
* 设置容器为特权模式，使得在容器中可以获取完全的root权限
* 设置容器的重启策略为一直重启
* 设置好容器的端口映射，根据需要的端口设置
* 若需要网桥，可设置好网桥
* 同一网桥下的容器彼此直接可以互联

```bash
docker run -itd -p 8020:8020 -p 9870:9870 --name=hadoop01 --hostname=hadoop01 --restart=always  --privileged=true centos:7 /usr/sbin/init
```

```bash
docker run -itd  -p 8088:8088 --name=hadoop02 --hostname=hadoop02 --restart=always  --privileged=true centos:7 /usr/sbin/init
```

```bash
docker run -itd -p 9868:9868 --name=hadoop03 --hostname=hadoop03 --restart=always  --privileged=true centos:7 /usr/sbin/init
```

