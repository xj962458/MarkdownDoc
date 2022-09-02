## 一、介绍

​        有时候创建了Docker容器，运行一段时间时候需要修改端口，若没有设置卷映射的话，重新创建会导致数据丢失等问题，即使创建了卷映射，也会比较麻烦。这里介绍一种不用删除容器，就可以直接更改Docker的映射端口，较为方便。



## 二、操作

### 1、停止容器

```bash
docker stop 容器ID
```


### 2、停止docker
```bash
systemctl stop docker
```


### 3、修改文件
​		**先进入目录**
```bash
cd /var/lib/docker/containers/容器ID*
```

#### （1）修改`hostconfig.json`文件

```bash
vi hostconfig.json
```

找到类似如下字段：

```json
"PortBindings": {
    "111/tcp": [  //容器端口1
        {
            "HostIp": "",
            "HostPort": "111" //宿柱机端口1
        }
    ],
    "222/tcp": [   //容器端口2
        {
            "HostIp": "",
            "HostPort": "222" //宿柱机端口2
        }
    ]
}
```

​		按照`json`文件格式添加或修改成需要的端口号即可



#### （2）修改`config.v2.json`文件

​		修改两处：

①、一处为：

```"ExposedPorts": {
"ExposedPorts": {
    "111/tcp": {},
    "222/tcp": {}
}
```

②、另一处为：

```json
"Ports": {
    "111/tcp": [
        {
            "HostIp": "0.0.0.0",
            "HostPort": "111"
        },
        {
            "HostIp": "::",
            "HostPort": "111"
        }
    ],
    "222/tcp": [
        {
            "HostIp": "0.0.0.0",
            "HostPort": "222"
        },
        {
            "HostIp": "::",
            "HostPort": "222"
        }
    ]
}
```

**和修改`hostconfig.json`文件类似，符合`json`语法格式即可**



### 4、重启`docker`

```bash
systemctl stop docker
```



### 5、重启容器

```bash
docker start 容器ID
```
