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
