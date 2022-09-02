# 一、安装内核

## 1、查看内核版本，命令

```bash
uname -r
```



## 2、执行以下命令

```bash
rpm -import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```



## 3、Centos 7.x 使用命令 

```bash
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```



## 4、好了到了这步，我们可以看下哪些最新内核是可以升级的，执行命令

```bash
yum --disablerepo="*"  --enablerepo="elrepo-kernel" list available
```



## 5、开始进行内核升级

### （1）直接升级到默认的最新稳定版内核

```bash
yum -y --enablerepo=elrepo-kernel install kernel-ml.x86_64 kernel-ml-devel.x86_64
```



### （2）自己选一个版本

```bash
yum -y --enablerepo=elrepo-kernel install kernel-lt.x86_64 kernel-lt-devel.x86_64
```



## 6、查看目前已经安装的内核版本

```bash
awk -F\' '$1=="menuentry " {print $2}' /etc/grub2.cfg
```



## 7、开始修改默认启动内核

```bash
vim /etc/default/grub
```

将 GRUB_DEFAULT=saved  改为 GRUB_DEFAULT=0 保存即可



## 8、通过grub2-mkconfig创建grub2配置文件

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```



## 9、重启

```bash
reboot
```



# 二、卸载内核

## 1、查看正在使用的内核

```bash
uname -a
```



## 2、查看系统全部内核

```bash
yum list installed *kernel*
```



## 3、删除多余内核

```bash
yum remove -y kernel-3.10.0-957.el7.x86_64
```



