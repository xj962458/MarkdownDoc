## 1、介绍

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用 `JupyterHub`，您可以创建一个多用户 Hub，它可以生成、管理和代理单用户 `Jupyter notebook` 服务器的多个实例。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该搭建教程基于docer容器版的`Debian`、`CentOS`系统，理论上`Ubuntu`、`Deepin`、`CentOS`等系统也适用.



## 2、安装必要的包

`Debain`系：

```bash
apt-get update -y && apt-get install vim procps nodejs npm wget -y
```



`RedHat`系：

```bash
yum update -y && yum install epel-release -y # 先安装个软件仓库
yum update -y && yum install vim nodejs npm wget -y
```



## 3、安装`Miniconda3`

下载地址：[Miniconda3清华镜像站下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)

选择较新版本，复制链接，然后：

```bash
# 下载
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh

# 赋予可执行权限
chmod +x ./Miniconda3-py39_4.12.0-Linux-x86_64.sh

# 执行安装
./Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

执行安装后，有几点注意：

* 刚执行的时候是服务条款，一路按`Enter`，然后要输入的时候输入`yes`即可；

* 接下来会让确认路径，默认为`~/miniconda3`，不要装在`root`用户目录下，建议装在`/opt/miniconda3`目录下，装`root`目录下之后会出现很多问题；

* 最后询问是否初始化，选择`yes`；

* 安装完成后重启终端，然后再进去，再进入发现终端`(base)`开头，说明安装成功

  ![image-20220912201812495](http://doc.xjfyt.top/markdown_img/20220912201813.png)



## 4、配置镜像源

```bash
npm config set registry http://registry.npmmirror.com  # npm换淘宝源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple # pip换清华源
```

```bash
# conda换源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/  
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/    
conda config --set show_channel_urls yes 
```

xxxxxxxxxx pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simplebash

## 5、安装configurable-http-proxy

```bash
npm install -g configurable-http-proxy
```



## 时区设置

```bash
dpkg-reconfigure tzdata
```



## 6、安装pycurl

```bash
conda install pycurl 
```

pycurl必须用conda装，用pip装会报错，或用源码编译安装，没有pycurl就会导致普通用户无法开启jupyterlab



## 7、安装`jupyter`相关的库

```bash
pip install jupyterlab jupyterhub jupyterhub-idle-culler jupyterlab-language-pack-zh-CN jupyterhub-dummyauthenticator
```

解释一下安装的包的含义：

* `jupyterlab`：`jupyter notebook`环境
* `jupyterhub`：`jupyterhub`主体程序
* `jupyterhub-idle-culler`：`用于处理用户空闲进程`
* `jupyterlab-language-pack-zh-CN`：`中文包`
* `jupyterhub-dummyauthenticator`：用户认证插件，CentOS下若不安装，无法登录jupyterlab



## 8、生成配置文件

```bash
mkdir /etc/jupyterhub/     # 先创建文件夹
jupyterhub --generate-config -f /etc/jupyterhub/jupyterhub_config.py
```



## 9、编辑配置文件

将一下内容追加到配置文件`/etc/jupyterhub/jupyterhub_config.py`中

```python
import sys

c.Authenticator.admin_users = {'root'}  # 管理员用户
# 管理员是否有权在各自计算机上以其他用户身份登录，以进行调试，此选项通常用于 JupyterHub 的托管部署，以避免在启动服务之前手动创建所有用户
c.JupyterHub.admin_access = True
# c.PAMAuthenticator.open_sessions = False # 解决多用户同时登录问题。此选项添加后用户用任何密码都可以登录
c.Spawner.args = ['--allow-root']  # 允许root用户使用
c.LocalAuthenticator.create_system_users = True  # 允许创建其他用户
c.Spawner.notebook_dir = '~'  # 设置工作目录
c.Spawner.default_url = '/lab'

# 设置用户一小时内无使用则关闭jupyterlab服务
c.JupyterHub.services = [
    {
        'name': 'idle-culler',
        'command': [sys.executable, '-m', 'jupyterhub_idle_culler', '--timeout=3600'],
    }
]

c.JupyterHub.load_roles = [
    {
        "name": "list-and-cull",  # name the role
        "services": [
            "idle-culler",  # assign the service to this role
        ],
        "scopes": [
            # declare what permissions the service should have
            "list:users",  # list users
            "read:users:activity",  # read user last-activity
            "admin:servers",  # start/stop servers
        ],
    }
]
```



依赖包

```bash
pip install autopep8 pycodestyle mccabe pycodestyle pydocstyle pyflakes pylint rope yapf whatthepatch
```



## 10、启动

```bash
nohup jupyterhub -f /etc/jupyterhub/jupyterhub_config.py > /etc/jupyterhub/jupyterhub.log 2>&1 &
```

可创建启动脚本，将上述代码写入，下次执行直接启动相应脚本即可



## 11、访问

```text
用户登录：http://IP:8000/hub/login
用户管理：http://IP:8000/hub/admin
```

**登录的密码是你系统用户的密码。若是要添加用户，在用户管理界面添加用户后，还需要在系统终端中修改密码：**

```bash
pssswd 用户
```



**登录界面**

![image-20220907084515904](http://doc.xjfyt.top/markdown_img/20220907084516.png)

**登陆后的jupyterlab界面：**

![image-20220907084711993](http://doc.xjfyt.top/markdown_img/20220907084712.png)



**管理界面：**

![image-20220907084603326](http://doc.xjfyt.top/markdown_img/20220907084603.png)
