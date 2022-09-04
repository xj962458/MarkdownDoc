## 1、常用端口

hadoop3.x：

* HDFS NameNode 内部常用端口：8020/9000/9820
* HDFS` NameNode 对用户查询端口：9870
* Yarn查看任务运行情况的：8088
* 历史服务器：19888

hadoop2.x：

* HDFS NameNode 内部常用端口：8020/9000
* HDFS` NameNode 对用户查询端口：50070
* Yarn查看任务运行情况的：8088
* 历史服务器：19888



## 2、常用配置文件

hadoop3.x:

* core-site.xml
* hdfs-site.xml
* yarn-site.xml
* mapred-site.xml
* workers

hadoop2.x:

* core-site.xml
* hdfs-site.xml
* yarn-site.xml
* mapred-site.xml
* slaves



## 3、Java连接

```bash
winutils # 在windows平台下使用hadoop的必要组件

hadoop-client # java中使用hadoop需要添加的依赖
```



## 4、hdfs基本命令

```bash
# 以下两种方法相同
hadoop fs 具体命令  
hdfs dfs 具体命令
```

除增加前缀外，和Linux中命令几乎一致

```bash
# 格式化NameNode
hdfs namenode -format
```

### （1） 输出命令参数

`-help` 

```bash
hadoop fs -help rm
```

### （2） 创建目录

`-mkdir` 

```bash
hadoop fs -mkdir /test
```
### （3）上传

* -moveFromLocal：从本地剪切到HDFS
```bash
hadoop fs -moveFromLocal ./hadoop.txt /test
```
* -copyFromLocal：从本地文件系统中拷贝文件到HDFS路径去
```bash
hadoop fs -copyFromLocal ./hadoop.txt /test
```
* -put 等同于copyFromLocal，生产环境更习惯用put
```bash
hadoop fs -put ./hadoop.txt /test
```
* -appendToFile：追加一个文件到已经存在的文件末尾
```bash
hadoop fs -appendToFile ./hadoop1.txt /test/hadoop.txt
```



### （4）下载
* -copyToLocal：从HDFS拷贝到本地
```bash
hadoop fs --copyToLoca /test/hadoop.txt ./hadoop.txt
```
* -get：等同于copyToLocal，生产环境更习惯用get
```bash
hadoop fs -get /test/hadoop.txt ./hadoop.txt
```

### （5）显示目录信息

`-ls`

```bash
hadoop fs -ls /test
```



### （6）显示文件内容

`-cat`

```bash
hadoop fs -cat /test/hadoop.txt
```



### （7）修改文件所属权限

`-chgrp、-chmod、-chown`，和Linux文件系统中的用法一样

```bash
hadoop fs -chmod -R 777 /test
```



### （8）从HDFS的一个路径拷贝到HDFS的另一个路径

`-cp`

```bash
hadoop fs -cp /test/hadoop.txt /hadoop.txt
```



### （9）在HDFS目录中移动文件

`-mv`

```bash
hadoop fs -mv /test/hadoop.txt /hadoop.txt
```



### （10）显示一个文件的末尾1kb的数据

`-tail`

```bash
hadoop fs -tail /hadoop.txt
```



### （11）删除文件或文件夹

`-rm`

```bash
hadoop fs -rm /hadoop.txt
```

可加`-r`参数



### （12）统计文件夹的大小信息

`-du`

```bash
hadoop fs -du -s -h /test
```



### （13）设置HDFS中文件的副本数量

`-setrep`

```bash
hadoop fs -setrep n /hadoop.txt
```

这里设置的副本数只是记录在NameNode的元数据中，是否真的会有这么多副本，还得看DataNode的数量。
