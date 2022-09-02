## 1、下载OpenCV源码

官网：[Releases - OpenCV](https://opencv.org/releases/)

![image-20220901152856932](http://doc.xjfyt.top/markdown_img/image-20220901152856932.png)



## 2、下载OpenCV-Contrib源码

GitHub地址：[opencv_contrib](https://github.com/opencv/opencv_contrib/tags)

**为什么要下载OpenCV contrib呢？**

​		因为自从OpenCV 3.0之后，很多经典的算法，比如sift和surf特征点检测算法，由于专利原因，已经不包含在OpenCV的源码当中了，需要下载OpenCV contrib包才能继续使用。

选择一个，点进去，此处选择的版本需要和上面下载的OpenCV版本一致：

![image-20220901153032349](http://doc.xjfyt.top/markdown_img/image-20220901153032349.png)

点击下载zip源码

![image-20220901153114335](http://doc.xjfyt.top/markdown_img/image-20220901153114335.png)



## 3、下载安装CMake

官方下载地址：[Download | CMake](https://cmake.org/download/)

![image-20220901153302218](http://doc.xjfyt.top/markdown_img/image-20220901153302218.png)



## 4、下载安装Visual Studio

官方下载地址：[Visual Studio 2022 IDE (microsoft.com)](https://visualstudio.microsoft.com/zh-hans/vs/)

最新版本是VS2022，社区版足够用，若需要安装专业版和企业版可网上寻找激活密钥

![image-20220901153558654](http://doc.xjfyt.top/markdown_img/image-20220901153558654.png)

下载的只是VS的安装程序，下载安装后启动，只选择`使用C++进行桌面开发即可`

![image-20220901153700445](http://doc.xjfyt.top/markdown_img/image-20220901153700445.png)



## 5、整理文件

源码下载后解压，为简单明了，将其放在一个文件夹中，并创建一个`build`文件夹，用来存放编译后的文件

![image-20220901153913639](http://doc.xjfyt.top/markdown_img/image-20220901153913639.png)



## 6、配置编译选项

![image-20220901154802779](http://doc.xjfyt.top/markdown_img/image-20220901154802779.png)

![image-20220901154951412](http://doc.xjfyt.top/markdown_img/image-20220901154951412.png)

​		需要保证有良好的网络环境，最好可以科学上网，要保证可以下载以`https://raw.githubusercontent.com`开头的文件，因为接下来会下载一些缺失的文件，均是从该链接下下载的，若没有良好的网络环境，需要看之后的修复过程。

等待一会，会出现以下界面，若为红色，可再点击一下`Configure`：

![image-20220902093840342](http://doc.xjfyt.top/markdown_img/image-20220902093840342.png)

**此处要在搜索框中搜索要勾选/取消勾选的内容如下：**

* `BUILD_opencv_world` 主要是把所有的lib文件都弄到一个opencv_world450d.lib中方便配置。

* `OPENCV_ENABLE_NONFREE` 是为了在编译成功后可以使用具有专利保护的算法，如果该变量不被选中，就不能使用具有专利保护的算法，使用`opencv-contrib`时务必勾选；

* `OPENCV_EXTRA_MODULES_PATH` ，选择`opencv_contrib`源码里的`modules`文件夹。如果这个变量为空，只是安装了OpenCV的基础版，不包括`opencv-contrib`；
* 输入`test` 找到`OPENCV_PERF_TESTS`、`BUILD_TESTS`、`BUILD_opencv_python_tests` **不勾选**；
* 输入`java` 查找`BUILD_JAVA`、`BUILD_opencv_java_bingdings_generator`**不勾选**，若需要`java`环境应勾选；
* 输入`python` 查找`BUILD_opencv_python3`、`BUILD_opencv_python_bingdings_generator`**不勾选**，若使用`Python`可以勾选，但若`Python`使用`OpenCV`，建议安装`opencv-python`和`opencv-contrib-python`，使用`pip`即可安装。

**完成上述步骤后，点击`Generate`**



## 7、使用VS进行编译生成

![image-20220901182639531](http://doc.xjfyt.top/markdown_img/image-20220901182639531.png)

点击`Generate`无错误完成后，再点击`Open Project`使用VS打开项目，右键点击解决方案，选择批生成：

![image-20220901182749181](http://doc.xjfyt.top/markdown_img/image-20220901182749181.png)

然后勾选这两项后，点击`生成`：

![image-20220901182932867](http://doc.xjfyt.top/markdown_img/image-20220901182932867.png)

等待一段时间，会生成完成，时间由你电脑性能决定，快的话十来分钟就好：

![image-20220901183015691](http://doc.xjfyt.top/markdown_img/image-20220901183015691.png)



然后找到刚才创建的`build`文件夹下，找到`install`文件夹，里面的内容即是编译好的文件。

![image-20220902084407904](http://doc.xjfyt.top/markdown_img/image-20220902084407904.png)



## 8、问题解决

​        在配置编译选项时，可能会出现很多报错情况，如下图所示，有的电脑虽然最后也显示配置完成了，但是也还是有很多文件没有安装完成，这在编译或后续的使用过程中可能会出现很多问题。

![image-20220902083712658](http://doc.xjfyt.top/markdown_img/image-20220902083712658.png)

​       错误的原因是因为国内网络无法正常从`github`中下载文件，所以需要我们手动下载文件，然后再复制到对应的文件夹中。

​	错误解决时，只需要注意这些下载错误即可，对于一下关于`Python`的缺失，可以不用管，因为并不编译和`Python`相关的`OpenCV`

### （1）`CMakeDownloadLog.txt`文件

​		该文件记录了一些文件的下载链接和下载后要存放的地址，如下图所示，`cmake_download`标志了哪一行是关于下载的，之后就是文件下载后要保存的位置，最后是下载地址。此处需要注意的是，保存的文件名和下载的文件名称不一样，保存的文件名称=下载文件的md5值+下载文件名。

![image-20220902081517121](http://doc.xjfyt.top/markdown_img/image-20220902081517121.png)

**注意：`CMake`中点击`Configure`和`Generate`均可能有文件下载，所以点击最好点击完后再去找`CMakeDownloadLog.txt`文件进行解析和下载文件。**



### （2）下载文件并移动

​		复制上面的下载地址，然后去下载，下载完成后重命名，然后移动到对应的文件夹，指定的路径可能存在文件，最好删除，因为那里面的文件大多是未下载完成的文件。

​		对于文件下载，若是之前运行`CMake`时报错，那么手动下载文件大概率也会无法下载或下载速度缓慢，此处需要用到`github`加速站来加速文件下载了。

有很多加速站可用，这里提供两个暂时可用的：

* [GitHub Proxy 代理加速 (ghproxy.com)](https://ghproxy.com/)

* [GitHub 文件加速 (91chi.fun)](https://github.91chi.fun/)

如下图所示，将文件下载链接输入到输入框中，点击下载即可加速下载。或直接在原链接前加上加速站的链接，如：

```bash
原下载链接：https://raw.githubusercontent.com/opencv/opencv_3rdparty/8afa57abc8229d611c4937165d20e2a2d9fc5a12/face_landmark_model.dat

加速后的下载链接：https://ghproxy.com/https://raw.githubusercontent.com/opencv/opencv_3rdparty/8afa57abc8229d611c4937165d20e2a2d9fc5a12/face_landmark_model.dat
```



![image-20220902082254383](http://doc.xjfyt.top/markdown_img/image-20220902082254383.png)



### （3）省事脚本

​		上述一点点复制文件下载链接再重命名是不是很麻烦，最后应该会下载十来个文件，正好电脑上有`Python`，又恰好会一点，写了个脚本，让脚本自动解析并下载文件，先将脚本贴上，运行脚本需下载`Python3`环境并安装`requests`库。

```python
```

