### 1、查看已开放的端口
```bash
firewall-cmd --list-ports
```


### 2、开启端口（以8080为例）
```bash
firewall-cmd --zone=public --add-port=8080/tcp --permanent
```

### 3、关闭端口
```bash
firewall-cmd --zone=public --remove-port=8080/tcp --permanent 
```

### 4、开启防火墙
```bash
systemctl start firewalld
```

### 5、关闭防火墙
```bash
systemctl stop firewalld
```

### 6、重启防火墙
```bash
# 以下两个任意一个即可
systemctl restart firewalld
firewall-cmd --reload
```

### 7、禁止开机启动
```bash
systemctl disable firewalld
```