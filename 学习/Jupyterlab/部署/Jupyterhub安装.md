## 一、安装docker

### 1、安装

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```



### 2、配置

```bash
# 重启docker
systemctl restart docker 

# 设置docker为开机自启动
systemctl enable docker
```



## 二、拉取镜像并创建容器

### 1、拉取镜像

```bash
docker pull debian:latest
```



### 2、创建容器

```bash
docker run -itd --hostname=jupyterhub -v /etc/jupyterhub:/etc/jupyterhub --name=jupyterhub --restart=always -p 8000:8000 debian
```

此处使用`debian`作为基本镜像进行搭建，也可以使用`ubuntu`和`centos等镜像`，对外暴露端口号为8000，若有端口冲突，可改为其他端口，`jupyterlab`服务运行在容器的8000端口上。



### 3、进入容器

创建完成后进入容器，接下来的众多命令均在容器内执行：

```bash
docker exec -it jupyterhub /bin/bash
```



## 三、安装必要的包

### 1、更新软件源

```bash
apt-get update -y && apt-get upgrade -y && apt-get autoremove -y
```



### 2、安装软件

```bash
apt-get install vim procps wget -y
```



## 四、安装`Miniconda3`

### 1、获取下载地址

下载地址：[Miniconda3清华镜像站下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)

选择较新版本，复制链接，

<img src="http://doc.xjfyt.top/markdown_img/20221106163015.png" alt="image-20221106163014071" style="zoom: 67%;" />



### 2、下载并安装

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

  <img src="http://doc.xjfyt.top/markdown_img/20221106152153.png" alt="image-20221106152152861" style="zoom:80%;" />

* 接下来会让确认路径，默认为`~/miniconda3`，不要装在`root`用户目录下，建议装在`/opt/miniconda3`目录下，装`root`目录下之后会出现很多问题；

  <img src="http://doc.xjfyt.top/markdown_img/20221106151945.png" alt="image-20221106151942693" style="zoom:80%;" />

* 最后询问是否初始化，选择`yes`；

  <img src="http://doc.xjfyt.top/markdown_img/20221106152032.png" alt="image-20221106152031667" style="zoom:80%;" />

* 安装完成后可删除安装包，以节省硬盘空间

  

### 3、验证是否安装成功

安装完成后重启终端，然后再进去，再进入发现终端`(base)`开头，说明安装成功

<img src="http://doc.xjfyt.top/markdown_img/20220912201813.png" alt="image-20220912201812495" style="zoom:80%;" />



## 五、安装nodejs和npm

`nodejs`和`npm`可以使用`apt-get`包管理工具进行安装:

```bash
apt install nodejs npm -y 
```

但版本可能不是很新，以下提供另一种安装方法：

### 1、下载

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



### 2、配置环境变量

```bash
vim /etc/bash.bashrc
```

追加以下内容：

```ini
#set for nodejs
export NODE_HOME=/opt/nodejs
export PATH=$NODE_HOME/bin:$PATH
```

使环境变量立即生效

```bash
source /etc/bash.bashrc
```



### 3、验证

`nodejs`和`npm`安装完成，使用以下命令进行验证

```bash
node -v # 查看node.js版本
npm -v  # 查看npm版本
```

![image-20220831153329582](http://doc.xjfyt.top/markdown_img/image-20220831153329582.png)





## 六、配置镜像源

npm、pip和conda的软件源都是国外的，在国内访问比较慢，换源有利于提高下载速度。

### 1、npm换源

```bash
npm config set registry http://registry.npmmirror.com  # npm换淘宝源
```



### 2、pip换源

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple # pip换清华源
```



### 3、conda换源

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/  
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/    
conda config --set show_channel_urls yes 
```



## 七、时区设置

不设置时区后续可能会出现一些问题。

终端执行：

```bash
dpkg-reconfigure tzdata
```

选择时区时选择`Asia/Shanghai`

![image-20221105102123476](http://doc.xjfyt.top/markdown_img/20221105102125.png)





##  八、安装`jupyter`相关的库

### 1、需要npm安装的

```bash
npm install -g configurable-http-proxy
```



### 2、需要conda安装的

```bash
conda install pycurl 
```

pycurl必须用conda装，用pip装会报错，或用源码编译安装，没有pycurl就会导致普通用户无法开启jupyterlab



### 3、需要pip安装的

```bash
pip install jupyterlab jupyterhub jupyterhub-idle-culler autopep8 pycodestyle mccabe pycodestyle pydocstyle pyflakes pylint rope yapf whatthepatch
```

解释一下安装的包的含义：

* `jupyterlab`：`jupyter notebook`环境
* `jupyterhub`：`jupyterhub`主体程序
* `jupyterhub-idle-culler`：`用于处理用户空闲进程`



## 九、配置jupyterhub

### 1、生成配置文件

```bash
jupyterhub --generate-config -f /etc/jupyterhub/jupyterhub_config.py
```



### 2、编辑配置文件

```bash
vim /etc/jupyterhub/jupyterhub_config.py
```



将以下内容追加到配置文件`/etc/jupyterhub/jupyterhub_config.py`中

```python
import sys

c.Authenticator.admin_users = {'root'}  # 管理员用户
# 管理员是否有权在各自计算机上以其他用户身份登录，以进行调试，此选项通常用于 JupyterHub 的托管部署，以避免在启动服务之前手动创建所有用户
c.JupyterHub.admin_access = True
c.PAMAuthenticator.open_sessions = False # 解决多用户同时登录问题。
c.Spawner.args = ['--allow-root']  # 允许root用户使用
c.LocalAuthenticator.create_system_users = True  # 允许创建其他用户
c.Spawner.notebook_dir = '~'  # 设置工作目录
c.Spawner.default_url = '/lab'

c.JupyterHub.extra_log_file = '/etc/jupyterhub/jupyterhub.log' # 指定额外的日志
c.JupyterHub.pid_file='/etc/jupyterhub/jupyterhub.pid' # 指定pid文件位置
c.JupyterHub.db_url='/etc/jupyterhub/jupyterhub.sqlite' # 指定数据库文件位置
c.JupyterHub.cookie_secret_file='/etc/jupyterhub/jupyterhub_cookie_secret'  # 指定cookie_secret文件位置
c.ConfigurableHTTPProxy.pid_file='/etc/jupyterhub/jupyterhub-proxy.pid' # 设置proxy.pid文件位置


# 设置用户一小时内无使用则关闭jupyterlab服务
c.JupyterHub.services = [
    {
        'name': 'idle-culler',
        'command': [sys.executable, '-m', 'jupyterhub_idle_culler', '--timeout=1800'],
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



## 十、启动

### （1）正常启动

创建启动脚本：

```bash
vim /etc/jupyterhub/start_jupyterhub.sh
```

写入以下内容：

```bash
nohup jupyterhub -f /etc/jupyterhub/jupyterhub_config.py > /dev/null 2>&1 &
```

赋予可执行权限：

```bash
chmod +x /etc/jupyterhub/start_jupyterhub.sh
```

**然后执行脚本启动`jupyterhub`**

```bash
/etc/jupyterhub/start_jupyterhub.sh
```



### （2）设置开机自启动

若每次启动都需要手动运行脚本来启动是在太麻烦，所以我们将脚本添加到容器的自启动中，让其随着容器的启动而启动。

容器在启动时，会先执行`/root/.bashrc`文件，我们将要执行的脚本加入其中即可实现jupyterhub随容器的启动而启动

```bash
vim /root/.bashrc
```

添加以下内容：

```bash
if [ -f /etc/jupyterhub/start_jupyterhub.sh ]; then
      /etc/jupyterhub/start_jupyterhub.sh
fi
```

添加完成后，我们可以退出容器，然后让容器重启，看重启后jupyterhub是不是自动启动了

```bash
# 重启容器
docker resatrt jupyterhub
```



## 十一、访问

### 1、设置密码

在访问前先设置密码，`root`用户为管理员，docker中的`root`用户默认是没有密码的，需要我们设置一个：

```bash
passwd root
```

![image-20221106153851365](http://doc.xjfyt.top/markdown_img/20221106153852.png)



### 2、登录

然后再进行登录

```text
用户登录：http://IP:8000/hub/login
用户管理：http://IP:8000/hub/admin
```

**登录的密码是你系统用户的密码。若是要添加用户，在用户管理界面添加用户后，还需要在系统终端中修改密码。**



**登录界面**：

<img src="http://doc.xjfyt.top/markdown_img/20221105103433.png" alt="image-20220907084515904" style="zoom:80%;" />

**登陆后的jupyterlab界面：**

![image-20220907084711993](http://doc.xjfyt.top/markdown_img/20221105103432.png)



**管理界面：**

![image-20221106154748959](http://doc.xjfyt.top/markdown_img/20221106154749.png)





## 十二、用户管理

### 1、单个或较少用户管理

#### （1）添加用户

使用`root`账户登录管理界面，然后点击`Add Users`添加用户，添加用户时候，每一行一个用户。可选择`Admin`设置添加的用户是否是管理员

![image-20221106154658257](http://doc.xjfyt.top/markdown_img/20221106154659.png)



添加后的界面如下：

![image-20221106154546811](http://doc.xjfyt.top/markdown_img/20221106154547.png)

#### （2）修改用户

添加用户后，可以点击`Edit User`进行用户的删除、修改用户名和赋予管理员权限等操作。

![image-20221106154920297](http://doc.xjfyt.top/markdown_img/20221106154921.png)



#### （3）修改和设置用户密码

`jupyterhub`无法在管理界面设置密码，设置密码需要在终端中进行设置。在`jupyterhub`终端中添加的用户，将被默认添加到系统用户中，并在`/home`文件夹下生成相应的用户目录：

![image-20221106155319549](http://doc.xjfyt.top/markdown_img/20221106155320.png)



因此，修改密码需要在终端中使用`passwd`命令来修改密码：

* root用户可直接使用`passwd 用户名`来修改密码，且修改密码不需要知道当前的密码：

  ![image-20221106155528300](http://doc.xjfyt.top/markdown_img/20221106155529.png)

* 普通用户只能够使用`passwd`修改自己的密码，且需要之前当前密码，密码也要设置8位及以上：

  ![image-20221106155956987](http://doc.xjfyt.top/markdown_img/20221106155958.png)

  

### 2、多用户批量管理

当需要有大量添加大量用户时，我们就需要使用`chpasswd`命令来批量修改密码

#### （1）添加用户

首先在管理面板中批量添加用户

![image-20221106160410224](http://doc.xjfyt.top/markdown_img/20221106160411.png)



#### （2）批量修改密码

然后将用户名和密码对应，写成`用户名:密码`的形式，存储在文件中，如存储在`passwd.txt`文件中，文件内容如下所示：

```text
lab01:3200201137
lab02:3200204233
lab03:3190707121
lab04:3200201232
lab05:3201902211
lab06:3200209116
lab07:3211902229
lab08:3211901113
lab09:3190113205
lab10:3210204416
lab11:3210204328
lab12:3210204326
lab13:3210204314
lab14:3210204428
lab15:3210201309
lab16:3200204317
lab17:3201901231
lab18:3201901103
lab19:3201901107
lab20:3210201225
```



然后在终端执行以下命令，即可完成用户密码的批量修改

```bash
chpasswd < passwd.txt
```



**这样添加的用户就可以通过用户名和设置的密码来访问了**



## 十三、jupyterlab功能扩展

jupyterhub是用来管理多用户使用jupyterlab，但我们实际去写代码的界面其实还是jupyterlab。初始的jupyterlab功能十分有限，没有代码提示和自动补全、没有代码自动保存、没有代码格式化，所以我们需要通过安装插件来补全这些功能。jupyterlab插件有很多，这里介绍几个常用的。

### 1、中文界面

默认的jupyterlab是英文界面，我们需要安装插件来中文化

```bash
pip install jupyterlab-language-pack-zh-CN
```

安装后需要配置

![image-20221106170330420](http://doc.xjfyt.top/markdown_img/20221106170331.png)



### 2、自动保存

自动保存功能不需要安装插件，且jupyterlab是开启的，但jupyterlab中自动保存间隔是120秒，我们需要修改这个值。

* **依次打开设置->高级设置编辑器->JSON设置编辑器**

![image-20221106170922030](http://doc.xjfyt.top/markdown_img/20221106170923.png)

* 然后添加自己的设置

![image-20221106171252840](http://doc.xjfyt.top/markdown_img/20221106171253.png)



### 3、自动闭合括号

设置->笔记本，勾选自动闭合括号

![image-20221106172343033](http://doc.xjfyt.top/markdown_img/20221106172344.png)



### 4、代码格式化

`jupyterlab_code_formatter`

  ```bash
  # 安装插件
  pip install jupyterlab_code_formatter
  # 安装格式化工具
  pip install black isort
  ```

该插件安装后需要重启才生效。

当我们写完代码后，点击如图所示的图标，代码就会被自动格式化：

![image-20221106173520468](http://doc.xjfyt.top/markdown_img/20221106173521.png)



### 5、树目录

`jupyterlab-unfold`

```bash
pip install jupyterlab-unfold
```



### 6、绘制可交互图

`jupyterlab-matplotlib`

```bash
pip install ipympl
```

该插件安装后，使用matplotlib绘图时只要加上以下代码，即可绘制可交互的图像：

```bash
%matplotlib widget
```

绘制的效果如下，可以点击图中的点查看对应数值，以及放大缩小图像等：

<img src="http://doc.xjfyt.top/markdown_img/20220913081831.png" alt="image-20220913081830624" style="zoom:80%;" />



### 7、代码补全和自动提示

```bash
pip install jupyterlab-lsp python-lsp-server
```

安装后重启，重启完成后进入到jupyterlab界面，然后进行设置。

进入设置，选择`Code Completion`，勾选`Continuous hinting`后刷新界面即可

![image-20221106172140939](http://doc.xjfyt.top/markdown_img/20221106172142.png)

效果如下，和Pycharm等IDE提供的代码提示类似：

![image-20221106173117277](http://doc.xjfyt.top/markdown_img/20221106173118.png)



### 8、代码执行时间

```bash
pip install jupyterlab_execute_time
```

如图所示，可以看到上次执行代码的时间和执行耗时：

![image-20221106173008870](http://doc.xjfyt.top/markdown_img/20221106173010.png)



### 9、绘制流程图

```bash
pip install ipydrawio
```

安装重启后，在开始页，可以看到增加了两个选项，点击可以创建绘图

![image-20221106172721122](http://doc.xjfyt.top/markdown_img/20221106172723.png)

创建的绘图界面如下，和drawio相似，其实就是drawio的jupyterlab插件：

![image-20221106172837726](http://doc.xjfyt.top/markdown_img/20221106172838.png)



### 10、多用户设置同步问题

* 当我们安装完插件后，需要进行一些设置才能够使用，但配置后我们发现只有当前用户可以使用，其他用户并没有进行配置。这是因为每个用户配置后相应的配置文件均保存在`~/.jupyter`文件夹下，若我们需要多个用户同时进行某项配置时，最简单的办法是直接使用配置好的用户的`.jupyter`文件夹替换要配置用户的文件夹即可，那样所有用户的配置都一样了。

* 值得注意的是，替换文件夹后要配置相应的权限，以免替换后被替换的用户无法访问配置文件而无法加载。最简单的方法是以下设置:

  ```bash
  chmod -R 777 .jupyter
  ```

* 当用户成百上千的时候，这么替换也属实麻烦，可以编写python或shell脚本去实现替换，以加快效率



## 十四、解决终端无法正常显示中文

### 1、安装locales

```bash
apt-get install locales -y
```



### 2、添加配置

```bash
dpkg-reconfigure locales
```
选择`zh_CN.UTF-8 UTF-8`
![20220907074812](http://doc.xjfyt.top/markdown_img/20220907074812.png)



### 3、查看语言设置

```bash
locale
```
保证`LANG`为`zh_CN.UTF-8 UTF-8`
![20220907074925](http://doc.xjfyt.top/markdown_img/20220907074925.png)

若不是，可以添加环境变量
```bash
export LANG=zh_CN.UTF-8
```
追加到`/etc/bash.bashrc`文件中，然后再使其生效
```bash
source /etc/bash.bashrc
```



### 4、效果

设置完成后重新打开终端，设置成功

**原来的显示**
![20220907075255](http://doc.xjfyt.top/markdown_img/20220907075255.png)
**现在的显示**
![20220907075323](http://doc.xjfyt.top/markdown_img/20220907075323.png)



## 十五、解决matplotlib绘图异常

### 1、问题描述

`matplotlib`绘图代码如下：

```python
import numpy as np
import matplotlib.pyplot as plt

plt.rcParams['font.family'] = ['sans-serif']
plt.rcParams['font.sans-serif'] = ['SimHei']

x=np.linspace(-np.pi,np.pi)
y1=np.sin(x)
y2=np.cos(x)
plt.title("常见三角函数")
plt.plot(x,y1,x,y2)
plt.show()
```

报错：

```text
Font family [‘sans-serif‘] not found.Falling back to DejaVu Sans
```

原因：系统中缺少`SimHei`字体



### 2、问题解决

#### （1）获取matplotlib的字体目录

```python
import patplotlib
print(matplotlib.matplotlib_fname())
```

```bash
/opt/miniconda3/lib/python3.9/site-packages/matplotlib/mpl-data/matplotlibrc
```



#### （2）打开字体目录

由上一步获取的地址修改得到

```bash
cd /opt/miniconda3/lib/python3.9/site-packages/matplotlib/mpl-data/fonts/ttf
```



####  （3）下载SimHei字体

下载地址：https://www.fontpalace.com/font-download/SimHei/

下载后复制到上一步得到的字体目录

![image-20220912174451218](http://doc.xjfyt.top/markdown_img/20220912174452.png)



#### （4）清除matplotlib缓存

```python
# 先获取缓存路径
import matplotlib
matplotlib.get_cachedir()
```

```bash
'/root/.cache/matplotlib'
```

```bash
# 清除缓存
rm -rf /root/.cache/matplotlib
```



#### （5）修改配置文件

配置文件即第一步获取的文件

```bash
vim /opt/miniconda3/lib/python3.9/site-packages/matplotlib/mpl-data/matplotlibrc
```

修改的几处如下：

```bash
# 删除前面的#号
font.family:  sans-serif

# 删除前面的#号，并在后面添加SimHei
font.serif:    SimHei, DejaVu Serif, Bitstream Vera Serif, Computer Modern Roman, New Century Schoolbook, Century Schoolbook L, Utopia, ITC Bookman, Bookman, Nimbus Roman No9 L, Times New Roman, Times, Palatino, Charter, serif

# 将True盖为False
axes.unicode_minus: False
```



#### （6）重启

重启后再运行没问题了



## 十六、内存占用问题

* 用户每打开一个Notebook文件，系统就会开始一个jupyter notebook内核进程，用户退出后进程不会自动终止；

* jupyterhub虽然可以自动释放资源，但并不会释放jupyter notebook内核进程；

* 当多个用户访问后，内存一直在增加，没有得到释放；

* 暂时没有好的解决方法，只能够让jupyterhub容器定时重启；

  我们使用Linux中的`crontab`命令设置定时重启`jupyterhub`容器，crontab是Linux系统下用于执行定时任务的一个工具，用法可以自行百度。

  ```bash
  crontab -e
  ```

  添加以下字段：

  ```bash
  30 2 * * * docker restart jupyterhub
  ```

  此字段指定每天凌晨2点30分重启jupyterhub容器
  
* 之后若有更好的办法再修改。
