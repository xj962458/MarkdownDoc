此处介绍Hadoop的分布式安装，安装步骤大致描述为：

* 安装`ssh`服务端和客户端，并配置`ssh`密钥，使各机器直接可以彼此无密码互联；
* 安装`JDK`，注意`JDK`的版本目前只能够为`JDK8`，其他版本会出现兼容问题；
* 安装`Hadoop`，从官网下载的压缩包，解压放置好后需要配置环境变量；
* 根据自身需求，修改`Hadoop`配置文件；
* 初次使用需要进行格式化，之后使用不需要；

## 1、准备工作

### （1）安装必备软件：

`CentOS`

```bash
yum update -y
yum install wget vim net-tools openssh-server openssh-clients -y
```

`Ubuntu`

```bash
apt update -y
apt install wget vim net-tools openssh-server openssh-clients -y
```



### （2）修改hostname和hosts

```bash
vim /etc/hostname
```

改成你要的主机名称，每台机器都要配置，如

```text
hadoop01 # 主机1
hadoop02 # 主机2
hadoop03 # 主机3
```



```bash
vim /etc/hosts
```

然后添加每天主机对于的ip地址

```bash
192.168.10.100 hadoop01
192.168.10.101 hadoop02
192.168.10.102 hadoop03
```



### （3）关闭防火墙

可以选择全部关闭，也可以选择打开端口，此处全部关闭

```bash
systemctl stop firewalld
systemctl disable  firewalld.service
```



## 2、`SSH`配置

`ssh`：secure shell

`sshd`：secure shell daemon



### （1）启动`ssh`服务并配置开机启动

```bash
# ssh客户端
systemctl start ssh && systemctl enable ssh

# ssh服务端
systemctl start sshd && systemctl enable sshd
```

在Ubuntu系统中需要修改`ssh`配置文件，允许`root`用户通过`ssh`登录：

```bash
vim /etc/ssh/sshd_config
```

添加内容：

```bash
PermitRootLogin yes
```

添加后重启`ssh`服务

```bash
systemctl restart sshd
```



### （2）配置密钥

```bash
ssh-keygen -t rsa
```

一直按`enter`，会在`~/.ssh`文件夹下生成`id_rsa`密钥和`id——rsa.pub`公钥

注意每台机器上都需要进行配置



### （3）添加公钥

```bash
ssh-copy-id 用户名@机器hostname或机器IP
```

然后需要输入密码，本机也要添加。



## 3、安装JDK

官网：[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/#java8)

若已有`JDK`要先卸载，`centos`卸载方法为

```bash
rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps 
```



下载需要登陆

![image-20220904100239231](http://doc.xjfyt.top/markdown_img/image-20220904100239231.png)

1. `CentOS`直接下载`x64 RPM Package`，然后使用`yum install 包名 -y`即可安装

​		安装后的目录为`/usr/java/jdk1.8.0_341-amd64`

2. `Ubuntu`等`Debian`系的系统，有多种安装方法，如：

	```bash
	apt-get install openjdk-8-jdk -y
	```

  		安装后的目录为`/usr/lib/jvm/java-1.8.0-openjdk-amd64`

 3. 再介绍一种通用安装方法

    官方下载界面选择下载`x64 Compressed Archive`，然后解压

    ```bash
    tar -zxvf jdk-8u341-linux-x64.tar.gz
    ```

    然后将其移动到指定位置

    ```bash
    mv ~/jdk1.8.0_341 /usr/java/jdk1.8.0_341
    ```

    然后配置环境变量

 4. 前两种不配置环境变量也能用，第三中不配置环境变量不行，一下介绍配置环境变量步骤

    ```bash
    vim /etc/profile
    ```

    添加以下内容：

    ```bash
    export JAVA_HOME=/usr/java/jdk1.8.0_341  # 此处换成自己解压的jdk 目录
    export JRE_HOME=$JAVA_HOME/jre  
    export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib  
    export PATH=$JAVA_HOME/bin:$PATH  
    ```

    ```bash
    source /etc/profile
    ```

    

    输入`java`和`javac`有输出就代表成功，`Hadoop`需要`JAVA_HOME`环境变量，因为无论哪种安装方法，都必须配置`JAVA_HOME`环境变量，否则会启动失败。配置完成后输入以下内容有输出说明配置完成：

    ```bash
    echo $JAVA_HOME
    ```



## 4、安装Hadoop

`Hadoop`官方下载地址：[Apache Hadoop](https://hadoop.apache.org/releases.html)

![image-20220904102557827](http://doc.xjfyt.top/markdown_img/image-20220904102557827.png)

下载后解压：

```bash
tar -zxvf hadoop-3.3.4.tar.gz
```

移动：

```bash
mv ~/hadoop-3.3.4 /usr/local/hadoop-3.3.4
```

添加环境变量

```bash
vim /etc/profile
```

添加以下内容

```bash
export HADOOP_HOME=/usr/local/hadoop-3.3.4
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

export HDFS_NAMENODE_USER=root
export HDFS_DATENODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root

export YARN_NODEMANAGER_USER=root
export YARN_RESOURCEMANAGER_USER=root
```

使其立即生效

```bash
source /etc/profile
```



## 5、修改`hadoop`配置文件

`core-site.xml`文件中添加

```w
<!-- 指定NameNode的地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop01:8020</value>
    </property>

    <!-- 指定hadoop数据的存储目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/hadoop-3.3.4/data</value>
    </property>
```

`hdfs-site.xml`文件中添加

```xml
<!-- nn web端访问地址-->
	<property>
        <name>dfs.namenode.http-address</name>
        <value>hadoop01:9870</value>
    </property>
	<!-- 2nn web端访问地址-->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>hadoop03:9868</value>
    </property>
```

`yarn-site.xml`文件中添加

```xml
<!-- 指定MR走shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!-- 指定ResourceManager的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop02</value>
    </property>

    <!-- 环境变量的继承 -->
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
```

`mapred-site.xml`文件中添加

```xml
<!-- 指定MapReduce程序运行在Yarn上 -->
<property>
    <name>mapredure.frameword.name</name>
    <value>yarn</value>
</property>
```



修改`workers`文件，添加所有节点，如：

```bash
hadoop01
hadoop02
hadoop03
```



## 6、启动

### （1）首次启动需要格式化格式化`NameNode`（hadoop01）

```bash
hdfs namenode -format
```



### （2）启动`HDFS`（hadoop01）

```bash
start-dfs.sh
```



### （3）启动`YARN`（hadoop02）

```bash
start-yarn.sh
```



### （4）Web端查看HDFS的`NameNode`

```bash
http://hadoop01:9870
```



### （5）Web端查看`YARN`的`ResourceManager`

```bash
http://hadoop02:8088
```



