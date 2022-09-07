## 一、安装cockpit相关软件包

```bash
yum install -y cockpit cockpit-docker cockpit-machines cockpit-dashboard cockpit-storaged cockpit-packagekit
```



## 二、启动服务

```bash
systemctl enable --now cockpit
```



## 三、访问服务

```bash
https:IP:9090
```

