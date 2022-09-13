## 一、问题描述

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



## 二、问题解决

### 1、获取matplotlib的字体目录

```python
import patplotlib
print(matplotlib.matplotlib_fname())
```

```bash
/opt/miniconda3/lib/python3.9/site-packages/matplotlib/mpl-data/matplotlibrc
```



### 2、打开字体目录

由上一步获取的地址修改得到

```bash
cd /opt/miniconda3/lib/python3.9/site-packages/matplotlib/mpl-data/fonts/ttf
```



###  3、下载SimHei字体

下载地址：https://www.fontpalace.com/font-download/SimHei/

下载后复制到上一步得到的字体目录

![image-20220912174451218](http://doc.xjfyt.top/markdown_img/20220912174452.png)



### 4、清除matplotlib缓存

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



### 5、修改配置文件

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



### 6、重启

重启后再运行没问题了