## 1、介绍

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用 *JupyterHub*，您可以创建一个多用户 Hub，它可以生成、管理和代理单用户 Jupyter notebook 服务器的多个实例。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该搭建教程基于docer容器版的`Debian`系统，理论上`Ubuntu`、`Deepin`等`Debian`系的系统也适用，`CentOS`不建议使用本教程，`CentOS`下搭建有权限问题未解决。



## 2、安装必要的包

```bash
apt-get update -y && apt-get install vim procps python3-pip python3-pycurl nodejs npm -y
```



## 3、配置镜像源

```bash
npm config set registry http://registry.npmmirror.com  # npm换淘宝源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple # pip换清华源
```



## 4、安装configurable-http-proxy

```bash
npm install -g configurable-http-proxy
```



## 5、安装`jupyter`相关的库

```bash
pip install jupyterlab jupyterhub jupyterhub-idle-culler jupyterlab-language-pack-zh-CN
```

解释一下安装的包的含义：

* `jupyterlab`：`jupyter notebook`环境
* `jupyterhub`：`jupyterhub`主体程序
* `jupyterhub-idle-culler`：`用于处理用户空闲进程`
* `jupyterlab-language-pack-zh-CN`：`中文包`



## 6、生成配置文件

```bash
jupyterhub -f /etc/jupyterhub/jupyterhub_config.py
```



## 7、编辑配置文件

将一下内容追加到配置文件`/etc/jupyterhub/jupyterhub_config.py`中

```python
import sys

c.Authenticator.admin_users = {'root'}  # 管理员用户
# 管理员是否有权在各自计算机上以其他用户身份登录，以进行调试，此选项通常用于 JupyterHub 的托管部署，以避免在启动服务之前手动创建所有用户
c.JupyterHub.admin_access = True
c.Spawner.args = ['--allow-root']  # 允许root用户使用
c.LocalAuthenticator.create_system_users = True  # 允许创建其他用户
c.Spawner.notebook_dir = '~'  # 设置工作目录
c.Spawner.default_url = '/lab'

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



## 8、启动

```bash
nohup jupyterhub -f /etc/jupyterhub/jupyterhub_config.py > /etc/jupyterhub/jupyterhub.log 2>&1 &
```

可创建启动脚本，将上述代码写入，下次执行直接启动相应脚本即可



## 9、访问

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
