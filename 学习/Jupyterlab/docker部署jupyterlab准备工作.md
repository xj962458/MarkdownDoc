## 1、拉取镜像并创建容器
```bash
docker run -itd --restart=always -p 18888:8888 --name=jupyterlab debian
```

此处使用`debian`作为基本镜像进行搭建，也可以使用`ubuntu`和`centos等镜像`，对外暴露端口号为18888，`jupyterlab`服务运行在容器的8888端口上。



## 2、进入容器

```bash
docker exec -it jupyterlab bash
```



## 3、更新软件源并安装软件

```bash
apt update -y && apt install wget procps vim -y
```



## 4、下载并安装`Miniconda3`

### （1）下载

可以从清华大学镜像站下载：[Minconda下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)

​        此处选择获取下载地址后使用`Linux`的`wget`命令进行下载，也可以先下载后再上传到`Linux`中。
​        选择要下载的版本（最好选较新的），然后点击右键，选择`复制链接`。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830202312.png)

然后使用命令：
```bash
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
```
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830202900.png)

### （2）安装
#### ① 首先赋予权限
```bash
chmod +x ./Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

#### ② 然后执行安装
```bash
./Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

执行安装程序后一直按`Enter`，需要输入的时候输入`yes`，最后快安装完成会出现是否初始化的提示，选择`yes`。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830203151.png)

然后重新打开终端，就可以看到终端前多个`base`，说明`conda`已经安装并初始化完成
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830203310.png)



## 5、`conda`和`pip`换源

换源是为了加快创建环境和安装第三方库的速度，此处使用清华源
### （1）`conda`换源
```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/  
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/    
conda config --set show_channel_urls yes 
```

### （2）`pip`换源
```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
