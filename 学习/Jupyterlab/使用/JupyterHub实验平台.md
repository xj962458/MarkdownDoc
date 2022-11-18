## 一、什么是`jupyter`

​		`Jupyter notebook`是一种 Web 应用，能让用户将说明文本、数学方程、代码和可视化内容全部组合到一个易于共享的文档中。它可以直接在代码旁写出叙述性文档，而不是另外编写单独的文档。也就是它可以能将代码、文档等这一切集中到一处，让用户一目了然。

​		Jupyter这个名字是它要服务的三种语言的缩写：Julia，Python和R，这个名字与“木星（jupiter）”谐音。`Jupyter Notebook` 已迅速成为数据分析，机器学习的必备工具。因为它可以让数据分析师集中精力向用户解释整个分析过程。我们可以通过Jupyter notebook写出了我们的学习笔记。但是jupyter远远不止支持上面的三种语言，目前能够使用的语言他基本上都能支持，包括C、C++、C#，java、Go等等。jupyter notebook和ipython其实都是来自同一个产品族，它的前身叫做ipython notebook，至于后面为什么更名这不得而知，这也就是为什么很多文章总是默认将`ipython`就说成是`ipython notebook`的原因了。

​		`jupyter lab`则是`jupyter notebook`的改进升级版。事实上，`JupyterLab` 是一个集 `Jupyter Notebook`、文本编辑器、终端以及各种个性化组件于一体的全能IDE。`jupyterlab`对文件的、代码和终端的管理更加便捷，所以此处主要使用`jupyterlab`而不是`jupyter notebook`，但两者大同小异，会一个，另一个自然也都会了。

Jupyter notebook的界面如下：

![image-20220912155247781](http://doc.xjfyt.top/markdown_img/20220912155248.png)

Jupyterlab的界面如下：

![image-20220912155331586](http://doc.xjfyt.top/markdown_img/20220912155332.png)



## 二、jupyter的作用

1. 极其适合数据分析，想象一下如下混乱的场景：你在终端中运行程序，可视化结果却显示在另一个窗口中，包含函数和类的脚本存在其他文档中，更可恶的是你还需另外写一份说明文档来解释程序如何执行以及结果如何。此时 Jupyter Notebook 从天而降，将所有内容收归一处，你是不是顿觉灵台清明，思路更加清晰了呢？

2. 支持多语言，也许你习惯使用 R 语言来做数据分析，或者是想用学术界常用的 MATLAB 和 Mathematica，这些都不成问题，只要安装相对应的核（kernel）即可。

3. 分享便捷，支持以网页的形式分享，GitHub 中天然支持 Notebook 展示，也可以通过 nbviewer 分享你的文档。当然也支持导出成 HTML、Markdown 、PDF 等多种格式的文档。

4. 远程运行，在任何地点都可以通过网络链接远程服务器来实现运算

5. 交互式展现，不仅可以输出图片、视频、数学公式，甚至可以呈现一些互动的可视化内容，比如可以缩放的地图或者是可以旋转的三维模型。这就需要交互式插件（Interactive widgets）来支持。

   

## 三、基本使用
###  1、界面介绍

**Jupyterlab**的主界面如下，整个界面可以分为三大区域，分别为：

* 文件管理区：最左边是代码区，在此区域内可以对文件进行管理，如打开文件、文件夹，切换`notebook`等操作；
* 代码区：中间是代码区，日常打开的notebook文件都在这里显示，此时未打开`notebook`文件，所以只显示启动页；
* 调试区：最右边是调试区，改区域可以对代码进行调试，观测代码运行时的各种参数；
* 菜单栏：最上方的是菜单栏，可以进行更复杂的操作；

文件管理区和调试区都是可以隐藏的，各区域的大小也可以通过拖动其边缘改变。

![image-20220912101400204](http://doc.xjfyt.top/markdown_img/20220912101401.png)



### 2、登录

登录地址为:[http://10.8.108.70:8000/hub/login](http://10.8.108.70:8000/hub/login)
打开后界面如下：

![image-20220918132836027](http://doc.xjfyt.top/markdown_img/20220918132837.png)

需要输入用户名和密码，用户名和密码说明如下：

* **人工智能专业：**用户名：ai20+序号，如序号为8，则用户名为`zi2008`
* **智能科学专业：**用户名：zk20+需要，如序号为20，则用户名为`zk2020`
* **初始密码：**各自学号
* 各自序号可查看任课教师点名册



登陆后的界面如下：

![image-20220918133945496](C:\Users\xjfyt\AppData\Roaming\Typora\typora-user-images\image-20220918133945496.png)





### 3、修改密码

初始密码为对于学号，若觉得密码不安全，可以修改密码，修改过程如下：

首先登录进去，然后新建终端

![image-20220918134141646](http://doc.xjfyt.top/markdown_img/20220918134142.png)



![image-20220918134158996](http://doc.xjfyt.top/markdown_img/20220918134159.png)



然后在终端中输入以下内容修改密码：

```bash
passwd
```

![image-20221104125247706](http://doc.xjfyt.top/markdown_img/20221104125301.png)



### 4、新建一个`notebook`

如下图所示，在启动页中可以快速新建`notebook`，还可以新建控制台、终端和其他文件。

![image-20220912101542960](http://doc.xjfyt.top/markdown_img/20220912101543.png)



除了启动页可以新建文件外，菜单栏也可以用来新建文件

![image-20220912101708229](http://doc.xjfyt.top/markdown_img/20220912101709.png)



新建的`notebook`如下：

* `notebook`文件的扩展名为`ipynb`，意为`ipython notebook`。
* 新建的文件默认名称为`Untitled.ipynb`，右键单击此文件可以进行重命名操作。
* 新建文件后，文件会默认打开，在代码区域可以看见文件，代码区可以同时打开多个`notebook`文件；
* 在代码去，打开的文件标题栏的右侧会显示一个原点，说明文件未保存，按住快捷键`Ctrl+S`即可快速保存，也可以通过菜单栏中的文件选项卡选择保存。

![image-20220912102038905](http://doc.xjfyt.top/markdown_img/20220912102039.png)





### 5、`notebook`的使用

#### （1）新建`Cell`

如图所示，点击`+`，可以新建一个`Cell`，可以将`Cell`认为是一块代码段，新建的Cell可以为代码、Markdown和纯文本。

* 代码：可以在其中编写Python代码，点击运行后可以显示运行结果，且有高亮显示；
* Markdown：通过Markdown语法编写文档，应用后会应用Markdown进行渲染，使界面更好看；
* 纯文本：所见即所得，写的是什么，显示的就是什么；
* 日常开发过程中，常用Markdown Cell来记录笔记，然后用代码Cell来显示代码和运行结果，相互配合，效率更高；

![image-20220912143137291](http://doc.xjfyt.top/markdown_img/20220912143138.png)



此外，当选中`Cell`的时候，还可以让其在代码、Markdown和纯文本之间切换

![image-20220912144159328](http://doc.xjfyt.top/markdown_img/20220912144200.png)



如下图所示，如三种Cell的演示

![image-20220912144732431](http://doc.xjfyt.top/markdown_img/20220912144733.png)





#### （2）运行Cell

`jupyter notebook`文件中，有两种`cell`是可以运行的，一种是代码`Cell`，运行后会在其下方产生运行输出，另一种是`Markdown Cell`，运行后会显示渲染后的效果。

* Cell 运行起来非常简单，就两步：①选中Cell；②运行Cell；

![image-20220912145141922](http://doc.xjfyt.top/markdown_img/20220912145143.png)



* 一个Cell运行完成后，会自动跳到下一个Cell中，所以可以一直点运行，运行所有的Cell；

* 当有多个Cell需要运行时，一个个的点实在太麻烦，此时可以使用菜单栏中`运行`选项进行批量运行

![image-20220912145426885](http://doc.xjfyt.top/markdown_img/20220912145427.png)





#### （3）Cell的其它操作

如下图所示，工具栏上，提供了Cell的保存、新建、删除、运行和停止等操作，要想要更多的操作，选选中Cell，然后单击右键，就可以运行更多的操作。

![image-20220912145706429](http://doc.xjfyt.top/markdown_img/20220912145707.png)



#### （4）常用快捷键

jupyterlab提供了很多快捷键，熟练使用快捷键可以提高效率，快捷键在选中Cell而不编辑时生效。常见的快捷键如下所示：

| command快捷键 | 描述 |
| --- | --- |
| A | 在上方插入新单元 |
| B | 在下方插入新单元 |
| C | 复制选中单元 |
| D,D | 删除选中单元 |
| F | 弹出’查找和替换’菜单 |
| H | 显示快捷键帮助 |
| I,I | 中断notebook内核 |
| 0,0 | 重启notebook内核 |
| J | 选中下方单元 |
| K | 选中上方单元 |
| L | 转换行号 |
| M | 单元转入markdown状态 |
| O | 转换输出 |
| Q | 关闭页面 |
| V | 粘贴到下方单元 |
| X | 剪切选中单元 |
| Y | 单元转入代码状态 |
| Z | 撤销上一步操作 |
| Up | 选中上方单元 |
| Down | 选中下方单元 |
| Enter | 转入编辑模式 |
| shift+上箭头/下箭头 | 选中多个单元 |
| shift+M | 合并选中单元 |
| shift+K | 扩大选中上方单元 |
| shift+J | 扩大选中下方单元 |
| shift+Enter | 运行本单元,选中下个单元 |
| Ctrl+S | 文件存盘 |
| Ctrl+Enter | 运行本单元 |
| Alt+Enter | 运行本单元,在其下插入新单元 |



### 6、绘图

#### （1）普通绘图

		在`jupyterlab`中使用`matplotlib`进行绘图，图片会自动嵌入到notebook中，显示在输出结果区域，而不是弹出一个窗口进行显示。在之前版本的`jupyter notebook`和`jupyterlab`中，要想让绘制的图片嵌入到`notebook`中，需要添加魔法命令`%matplotlib inline`，但在如今的版本中，不添加该命令，也可以自动嵌入到`notebook`中。

如下图所示，左边是未添加魔法命令绘制的图片，右边是添加了魔法命令绘制的图片，并无区别；但当遇见绘制的图片无法在`notebook`中正常显示时，需要添加以下代码

```python
%matplotlib inline
```

<center class="half">
<img src="http://doc.xjfyt.top/markdown_img/20220913075359.png" alt="image-20220913075359040" style="zoom:53.5%;" />
<img src="http://doc.xjfyt.top/markdown_img/20220913075557.png" alt="image-20220913075556574" style="zoom: 53.5%;" />
</center>


#### （2）交互式绘图

在`jupyterlab`中，可以以一种交互式的方式来进行绘图，主要依赖` jupyter-matplotlib`插件

其官方文档为：https://github.com/QuantStack/jupyterlab-drawio

交互式绘图也需要一个魔法命令，为

```python
%matplotlib widget
```

绘图效果如下，看似和普通绘图没有区别，但当我们点击图像时，就发现多了几个按钮，通过这些按钮，我们可以与图交互；

<img src="http://doc.xjfyt.top/markdown_img/20221104124649.png" alt="image-20220913081409674" style="zoom: 80%;" />



如下图所示，当我们的鼠标点击图像时，下方会显示此时点击的坐标。左侧有六个按钮，通过这六个按钮，我们可以进行更近一步的交互，如放大图像细节、保存图像等操作。

![image-20220913081830624](http://doc.xjfyt.top/markdown_img/20221104124649.png)

#### （3）绘制流程图

在`jupyterlab`中可以绘制流程图，但不是通过代码，而是通过鼠标和键盘操作，这依赖于`jupyterlab`的另一个插件`jupyterlab-drawio`，其官方文档为：[https://github.com/QuantStack/jupyterlab-drawio](https://github.com/QuantStack/jupyterlab-drawio)

打开启动页，选择`Other`->`Diagram`就可以创建一个图像

![image-20220913082737048](http://doc.xjfyt.top/markdown_img/20221104124649.png)

看起来是不是很熟悉，是的，就是和网页版的`drawio`一样，在上面的使用也和`drawio`一样

![image-20220913082837859](http://doc.xjfyt.top/markdown_img/20221104124649.png)



### 7、调试代码

#### （1）建立notebook

​		最初使用`jupyterlab`调试代码，需要选择`XPython`内核才能够进行调试，现在在配置好调试环境后，使用普通内核和`XPython`内核均可进行调试，如下图所示，在建立`notebook`时，选用一下两种内核中的任意一种均可，但不要选择中间的内核，中间的内核不支持调试功能。

![image-20220912185725939](http://doc.xjfyt.top/markdown_img/20220912185726.png)

若已创建`notebook`，可点击切换内核，如下图所示

![image-20220912185926783](http://doc.xjfyt.top/markdown_img/20220912185927.png)



#### （2）编写简单的调试代码

Cell1

```python
def add(a,b):
    res=a+b
    return res
```



Cell2

```bash
for i in range(10,20,4):
    res=add(i,i*i)
    print(res)
```

![image-20220912190446244](http://doc.xjfyt.top/markdown_img/20220912190447.png)

该段代码是输出几组两数之和，通过调试，我们来观察它的运行过程



#### （3）配置调试

配置调试共分为四步，如下图所示：

* 打开调试窗口：打开调试窗口后才能够看到各种调试信息，如图所示，调试窗口内由变量、调用堆栈、断点、源文件和内核源等区域，其中变量区域显示调试过程中的变量，现在上面显示有值，这是上次调试遗留在内存中的值，不影响之后的调试结果；调用堆栈出现在是空白的，等调试的时候，我们通过堆栈来控制调试的过程。断点处有两条，是我们添加的断点；源文件处现在也是空白，当调试时它会显示正在调试的源代码。
* 打开调试开关：要进行调试，需要打开调试开关，进行调试模式，有些内核不支持调试，则此开关无法点击；
* 添加断点：添加断点是用于找出程序的故障，在调试的过程中，会在断点处暂停，从而判断哪出有问题；
* 开始调试：在非调试模式下，该按钮是运行操作，在调试模式下，它是调试操作。在此调试中，在两个代码Cell中均需要点击调试按钮，然后才开始调试，因为第一个Cell是一个函数，第二个Cell才是运行的程序；

![image-20220912190913696](http://doc.xjfyt.top/markdown_img/20220912190914.png)



#### （4）开始调试

​		调试过程如下图所示，在两个Cell中点击完调试按钮后，我们来到`调用堆栈`区域，通过点击`下一步`按钮，来控制代码一点点运行，在运行的过程中，左边的代码区域中带阴影的行表明现在所处的位置，在`源文件`区域也可以查看；除此之外，在调试过程中，`变量`区域的值也在不断变化，因为不同的值参与了求和。

​		这是最简单的调试过程，通过这钟调试，可以加深对代码运行过程的理解，更容易找出代码中的错误。

![动画](http://doc.xjfyt.top/markdown_img/20220912192340.gif)



### 8、常见命令操作

#### （1）内嵌帮助

		在代码单元中，如果不知道某个函数的用法或者命令的用法，可以在其后敲入问好?然后运行，系统就会显示帮助文档。比如敲入pirnt?，然后运行该cell，则会显示内嵌的帮助文档。

![image-20220912151222227](http://doc.xjfyt.top/markdown_img/20221104125417.png)

还可以通过help()来启动交互式帮助控制台，需要注意的是，进入交互控制台后，无法运行其他Cell，需要先终止该Cell，才能再运行别的Cell。

![image-20220912151627071](http://doc.xjfyt.top/markdown_img/20221104125417.png)

![image-20220912151643435](http://doc.xjfyt.top/markdown_img/20221104125417.png)





#### （2）Shell命令

在代码单元中，可以执行shell命令。shell命令以`!`开头，然后加上要执行的命令即可，如下所示。

![image-20220912152348698](http://doc.xjfyt.top/markdown_img/20221104125417.png)





#### （3）magic命令

jupyter notebook的代码单元还支持一种以%开头的称为magic的命令。这些命令包括一些内嵌的工具，如测量时间的`%timeit`，还有一些shell的功能，如%cat等。同样，可以在magic命令后面加?来显示使用帮助。下面是几个示例。

* `%timeit`

如下图所示，应用该命令查看了运行`range(1000)`代码所需时间

![image-20220912153035155](http://doc.xjfyt.top/markdown_img/20221104125417.png)

* `%matplotlib inline`

  该命令是内嵌绘图操作，使用该条命令后，使用`matplotlib`绘制图像时，不用添加`plt.show()`即可在终端中正常显示绘制的图像。有些版本的jupyter notebook不使用这条命令，绘制的图是无法在notebook里面现实的，但最新版本的jupyterlab无论是否使用该命令，都会在`notebook`中显示图像。

  ![image-20220912153322660](http://doc.xjfyt.top/markdown_img/20221104125417.png)

* `%cat`

  用来显示文件内容

  ![image-20220912153659232](http://doc.xjfyt.top/markdown_img/20221104125417.png)

* `%magic`

  该命令可以查看全部的magic命令

  ![image-20220912153819507](http://doc.xjfyt.top/markdown_img/20221104125417.png)