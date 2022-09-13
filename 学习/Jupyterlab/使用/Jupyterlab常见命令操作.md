## 1、内嵌帮助

​		在代码单元中，如果不知道某个函数的用法或者命令的用法，可以在其后敲入问好?然后运行，系统就会显示帮助文档。比如敲入pirnt?，然后运行该cell，则会显示内嵌的帮助文档。

![image-20220912151222227](http://doc.xjfyt.top/markdown_img/20220912151223.png)

还可以通过help()来启动交互式帮助控制台，需要注意的是，进入交互控制台后，无法运行其他Cell，需要先终止该Cell，才能再运行别的Cell。

![image-20220912151627071](http://doc.xjfyt.top/markdown_img/20220912151627.png)

![image-20220912151643435](http://doc.xjfyt.top/markdown_img/20220912151644.png)





## 2、Shell命令

在代码单元中，可以执行shell命令。shell命令以`!`开头，然后加上要执行的命令即可，如下所示。

![image-20220912152348698](http://doc.xjfyt.top/markdown_img/20220912152349.png)





## 3、magic命令

jupyter notebook的代码单元还支持一种以%开头的称为magic的命令。这些命令包括一些内嵌的工具，如测量时间的`%timeit`，还有一些shell的功能，如%cat等。同样，可以在magic命令后面加?来显示使用帮助。下面是几个示例。

* `%timeit`

如下图所示，应用该命令查看了运行`range(1000)`代码所需时间

![image-20220912153035155](http://doc.xjfyt.top/markdown_img/20220912153035.png)

* `%matplotlib inline`

  该命令是内嵌绘图操作，使用该条命令后，使用`matplotlib`绘制图像时，不用添加`plt.show()`即可在终端中正常显示绘制的图像。有些版本的jupyter notebook不使用这条命令，绘制的图是无法在notebook里面现实的，但最新版本的jupyterlab无论是否使用该命令，都会在`notebook`中显示图像。

  ![image-20220912153322660](http://doc.xjfyt.top/markdown_img/20220912153323.png)

* `%cat`

  用来显示文件内容

  ![image-20220912153659232](http://doc.xjfyt.top/markdown_img/20220912153700.png)

* `%magic`

  该命令可以查看全部的magic命令

  ![image-20220912153819507](http://doc.xjfyt.top/markdown_img/20220912153820.png)