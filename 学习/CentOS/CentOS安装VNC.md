查看是否安装vnc
```
rpm -qa | grep vnc
```


centos安装vnc
```bah
yum install vnc vnc-server 
yum install tightvnc-server
```

去除端口防火墙

```bash
iptables -I INPUT -p tcp --dport 5901 -j ACCEPT 
```

查看是否安装Gnome桌面环境【vnc viewer连接黑屏可能没安装这个桌面环境】

```bash
yum grouplist
```

安装Gnome桌面环境

```bash
yum groupinstall "GNOME Desktop"
```

新建桌面:1

vncserver :1

输入vncserver :1后设置密码  
Password: 设置使用VNC连接服务器的密码  
Verify: 再次输入一遍密码  
Would you like to enter a view-only password (y/n)?  选择n，以使连接到服务器后可以操作服务器

**其他命令：**

设置VNC密码

vncpasswd

查看vncserver的服务情况

vncserver -list

开启编号为:1的桌面

vncserver :1

关闭编号为:1的vncserver

vncserver -kill :1

更新systemctl使配置生效

systemctl daemon-reload

 客户端黑屏  CentOS 执行：chmod 755 /root/.vnc/xstartup

卸载vnc软件

yum remove tigervnc-server -y

删除vnc下的配置

rm -rf /root/.vnc
rm -rf /etc/.X11-unit
rm -rf /etc/.X*-lock

win10 使用 vnc viewer 连接 linxu 的vnc server