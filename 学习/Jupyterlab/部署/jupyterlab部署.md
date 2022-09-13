## 1、准备

​       本教程是适用于`Linux`操作系统的`jupyterlab`部署教程，需要提前安装`Python3`运行环境，可以是基本的`Python3`、`Anaconda`和`MiniConda`等`Python3`环境，需要有良好的网络环境。

​		若使用`docker`安装，准备过程可以借鉴[docker部署jupyterlab准备工作.md](./docker部署jupyterlab准备工作.md)



## 2、安装`jupyterlab`

```bash
pip install jupyterlab # 安装jupyter
pip install jupyterlab-language-pack-zh-CN # 安装汉化包，需要自己选择语言
```



## 3、生成配置文件

```bash
jupyter lab --generate-config
```

上面命令会生成`jupyterlab`配置文件，路径为`~/.jupyter/jupyter_lab_config.py`



## 4、创建密码

```bash
jupyter lab password
```

填写密码并确认密码，会生成`~/.jupyter/jupyter_server_config.json`

查看文件内容，如下图，复制`password`后的一串字符
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830215828.png)



## 5、修改配置文件

```bash
vim  ~/.jupyter/jupyter_lab_config.py
```
追加以下内容：
```python
c.ServerApp.allow_remote_access = True  # 允许远程访问
c.ServerApp.allow_root = True           # 允许root运行
c.ServerApp.notebook_dir = u'工作文件夹'  # 设置工作目录，默认为用户家目录
c.ServerApp.ip = '*'                    # 监听地址
c.ServerApp.port = 8888                 # 运行端口，默认8888
c.ServerApp.password = '刚复制的字符串'    # 密码
c.ServerApp.open_browser = False        # 不打开浏览器
```



## 6、后台启动

```bash
nohup python -m jupyterlab --allow-root > ~/.jupyter/jupyter.log 2>&1 &
```
​       此处利用`nohup`命令让`jupyterlab`在后台执行，并输出日志到`~/.jupyter/jupyter.log`文件中，该串代码运行后会输出`jupyterlab`的进程号，通过进程号，可以使用`kill -9 进程号`来终止项目，如下图，`4094`即是此次`jupyterlab`的进程号，使用`kill -9 4094`即可停止后台运行的`jupyterlab`。
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830221534.png)

启动后，通过`IP:端口号`即可进行访问，访问后需要输入之前设置的密码，然后进入的主页面如图所示：

![image-20220831152742285](http://doc.xjfyt.top/markdown_img/image-20220831152742285.png)



## 7、终止后台运行的`jupyterlab`

​       使用`ps -al`命令查看进程号，即`PID`，然后使用`kill`命令终止。若想重新启动，再重新运行命令
即可。

```bash
(base) [root@0ca82869d78e ~]# ps -al #查看进程
F S   UID    PID   PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0 184175 184005  1  80   0 - 90168 ep_pol pts/2    00:00:03 python
0 R     0 184429 184005  0  80   0 - 12402 -      pts/2    00:00:00 ps
```

```bash
kill -9 184175 # 结束进程
```



## 8、切换中文界面

若需中文，使用以下命令安装中文包后，在`jupyterlab`界面中切换
![](http://doc.xjfyt.top/markdown_img/Pasted%20image%2020220830222127.png)



## 9、安装扩展

​       `jupyterlab`相比于传统的`jupyter notebook`，最大的变化在于支持安装扩展，但很多扩展的安装需要先安装`node.js`和`npm`，以下介绍如何安装，若需求较少，可以跳过此步骤。

### （1）下载并安装`Nodejs`

#### ① 下载

`node.js`中文官网：[下载 | Node.js 中文网 (nodejs.cn)](http://nodejs.cn/download/)

![image-20220831091237436](http://doc.xjfyt.top/markdown_img/image-20220831091237436.png)

访问界面，选择`Linux 二进制文件 (x64)`，右键`复制链接`

返回终端，下载：

```bash
wget https://nodejs.org/dist/v16.17.0/node-v16.17.0-linux-x64.tar.xz
```

解压：

```bash
tar -xvf ./node-v16.17.0-linux-x64.tar.xz
```

移动并重命名

```bash
mv node-v16.17.0-linux-x64/ /opt/nodejs
```



#### ② 配置环境变量

```bash
vim /etc/profile
```

追加以下内容：

```ini
#set for nodejs
export NODE_HOME=/opt/nodejs
export PATH=$NODE_HOME/bin:$PATH
```

使环境变量立即生效

```bash
source /etc/profile
```



#### ③ 验证

`nodejs`和`npm`安装完成，使用以下命令进行验证

```bash
node -v # 查看node.js版本
npm -v  # 查看npm版本
```

![image-20220831153329582](http://doc.xjfyt.top/markdown_img/image-20220831153329582.png)



#### ④ 换源

若按照速度较慢，可进行换源，此处换成了淘宝源

```bash
npm config set registry http://registry.npmmirror.com
```

若想换回官方源：

```bash
npm config set registry https://registry.npmjs.org/
```

清除缓存

```bash
npm cache clean --force
```



### （2）安装扩展

如图所示，在`jupyterlab`界面的右侧即可进行扩展安装

![image-20220831153825545](http://doc.xjfyt.top/markdown_img/image-20220831153825545.png)



## 10、插件推荐

* ipydrawio

  drawio的jupyterlab插件

  ```text
  pip install ipydrawio
  ```

![image-20220912202901697](http://doc.xjfyt.top/markdown_img/20220912202902.png)



## 11、其他问题解决

**问题描述**

```bash
ImportError: IProgress not found. Please update jupyter and ipywidgets. 
See https://ipywidgets.readthedocs.io/en/stable/user_install.html
```

### （1）安装依赖

```bash
pip install ipywidgets widgetsnbextension pandas-profiling
```
### （2）启动​​`jupyterlab`​​相应的插件

```bash
jupyter nbextension enable --py widgetsnbextension
```
