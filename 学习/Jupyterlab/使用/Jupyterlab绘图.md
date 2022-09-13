## 1、普通绘图

​		在`jupyterlab`中使用`matplotlib`进行绘图，图片会自动嵌入到notebook中，显示在输出结果区域，而不是弹出一个窗口进行显示。在之前版本的`jupyter notebook`和`jupyterlab`中，要想让绘制的图片嵌入到`notebook`中，需要添加魔法命令`%matplotlib inline`，但在如今的版本中，不添加该命令，也可以自动嵌入到`notebook`中。

如下图所示，左边是未添加魔法命令绘制的图片，右边是添加了魔法命令绘制的图片，并无区别；但当遇见绘制的图片无法在`notebook`中正常显示时，需要添加以下代码

```python
%matplotlib inline
```

<center class="half">
<img src="http://doc.xjfyt.top/markdown_img/20220913075359.png" alt="image-20220913075359040" style="zoom:53.5%;" />
<img src="http://doc.xjfyt.top/markdown_img/20220913075557.png" alt="image-20220913075556574" style="zoom: 53.5%;" />
</center>



## 2、交互式绘图

在`jupyterlab`中，可以以一种交互式的方式来进行绘图，主要依赖` jupyter-matplotlib`插件

其官方文档为：https://github.com/QuantStack/jupyterlab-drawio

交互式绘图也需要一个魔法命令，为

```python
%matplotlib widget
```

绘图效果如下，看似和普通绘图没有区别，但当我们点击图像时，就发现多了几个按钮，通过这些按钮，我们可以与图交互；

<img src="http://doc.xjfyt.top/markdown_img/20220913081410.png" alt="image-20220913081409674" style="zoom: 80%;" />



如下图所示，当我们的鼠标点击图像时，下方会显示此时点击的坐标。左侧有六个按钮，通过这六个按钮，我们可以进行更近一步的交互，如放大图像细节、保存图像等操作。

![image-20220913081830624](http://doc.xjfyt.top/markdown_img/20220913081831.png)

## 3、绘制流程图

在`jupyterlab`中可以绘制流程图，但不是通过代码，而是通过鼠标和键盘操作，这依赖于`jupyterlab`的另一个插件`jupyterlab-drawio`，其官方文档为：[https://github.com/QuantStack/jupyterlab-drawio](https://github.com/QuantStack/jupyterlab-drawio)

打开启动页，选择`Other`->`Diagram`就可以创建一个图像

![image-20220913082737048](http://doc.xjfyt.top/markdown_img/20220913082738.png)

看起来是不是很熟悉，是的，就是和网页版的`drawio`一样，在上面的使用也和`drawio`一样

![image-20220913082837859](http://doc.xjfyt.top/markdown_img/20220913082838.png)