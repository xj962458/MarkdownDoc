## 一、界面介绍

**Jupyterlab**的主界面如下，整个界面可以分为三大区域，分别为：

* 文件管理区：最左边是代码区，在此区域内可以对文件进行管理，如打开文件、文件夹，切换`notebook`等操作；
* 代码区：中间是代码区，日常打开的notebook文件都在这里显示，此时未打开`notebook`文件，所以只显示启动页；
* 调试区：最右边是调试区，改区域可以对代码进行调试，观测代码运行时的各种参数；
* 菜单栏：最上方的是菜单栏，可以进行更复杂的操作；

文件管理区和调试区都是可以隐藏的，各区域的大小也可以通过拖动其边缘改变。

![image-20220912101400204](http://doc.xjfyt.top/markdown_img/20220912101401.png)



## 二、新建一个`notebook`

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





## 三、`notebook`的使用

### 1、新建`Cell`

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





### 2、运行Cell

`jupyter notebook`文件中，有两种`cell`是可以运行的，一种是代码`Cell`，运行后会在其下方产生运行输出，另一种是`Markdown Cell`，运行后会显示渲染后的效果。

* Cell 运行起来非常简单，就两步：①选中Cell；②运行Cell；

![image-20220912145141922](http://doc.xjfyt.top/markdown_img/20220912145143.png)



* 一个Cell运行完成后，会自动跳到下一个Cell中，所以可以一直点运行，运行所有的Cell；

* 当有多个Cell需要运行时，一个个的点实在太麻烦，此时可以使用菜单栏中`运行`选项进行批量运行

![image-20220912145426885](http://doc.xjfyt.top/markdown_img/20220912145427.png)





### 3、Cell的其它操作

如下图所示，工具栏上，提供了Cell的保存、新建、删除、运行和停止等操作，要想要更多的操作，选选中Cell，然后单击右键，就可以运行更多的操作。

![image-20220912145706429](http://doc.xjfyt.top/markdown_img/20220912145707.png)



### 4、常用快捷键

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