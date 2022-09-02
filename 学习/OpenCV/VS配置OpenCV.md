## 1、创建项目

打开VS，创建项目，空项目和控制台项目都行，空项目不会创建`cpp`文件，需要自己重新添加。

![image-20220902084728057](http://doc.xjfyt.top/markdown_img/image-20220902084728057.png)



## 2、配置

以下主要介绍配置`Debug`时的操作，配置`Release`时的操作和`Debug`时的操作除了配置链接器那儿有一点不同之外，其余都是一样，此处不多介绍。

### （1）配置环境变量

右键`我的电脑`，选择`属性`

![image-20220902090019421](http://doc.xjfyt.top/markdown_img/image-20220902090019421.png)

然后依次点击`高级系统设置`、`环境变量`、`Path`

![image-20220902090157068](http://doc.xjfyt.top/markdown_img/image-20220902090157068.png)

然后点击新建，将OpenCV目录下的`x64/vc**/bin`添加到其中，然后一个个点击确定

![image-20220902090347859](http://doc.xjfyt.top/markdown_img/image-20220902090347859.png)



### （2）配置`VC++`目录

如下图所示，确定当前是为`Dehug`配置，然后右键点击项目名称，选择属性，注意点击项目名称，而不是解决方案名称。

![image-20220902084842802](http://doc.xjfyt.top/markdown_img/image-20220902084842802.png)

然后选择`VC++目录`,选择包含目录和库目录两个选项，然后点编辑

![image-20220902085115761](http://doc.xjfyt.top/markdown_img/image-20220902085115761.png)

新建路径，然后点击`...`选择路径：

* `VC++目录`设置为`OpenCV`中的`include`文件夹
* `库目录`设置为`OpenCV`中的`X64/V**/lib`文件夹，注意此处的`V**`因为版本不同，可能不同，若`X64`文件夹下存在多个`V**`的文件夹，选择更新的即可。

![image-20220902085234459](http://doc.xjfyt.top/markdown_img/image-20220902085234459.png)

我的配置好的如下图所示，下载的OpenCV版本不同，可能略微有差异，但总体类似。

![image-20220902085725131](http://doc.xjfyt.top/markdown_img/image-20220902085725131.png)



### （3）配置链接器

打开`OpenCV`所在文件夹，然后找到`x64/v**/lib`文件夹，该文件夹下可能有多个`*.lib`文件夹，需要复制其文件名。

此处需要注意的是，为`DeBug`配置，需要找到`*d.lib`文件，如下图，只需要添加`opencv_world460d.lib`即可，为`Release`配置的话，只需要找到`*.lib`文件，此处为`opencv_world460d.lib`，`d`的意思为`debug`。

此处只有两个`lib`文件，有些`OpenCV`版本可能有多个`*.lib`文件，都需要进行添加。

![image-20220902090527247](http://doc.xjfyt.top/markdown_img/image-20220902090527247.png)

![image-20220902092942587](http://doc.xjfyt.top/markdown_img/image-20220902092942587.png)

![image-20220902093013396](http://doc.xjfyt.top/markdown_img/image-20220902093013396.png)



## 3、测试代码

该测试代码是打开摄像头

```cpp
#include <openCV2/opencv.hpp>
#include <iostream>
using namespace cv;
int main()
{
    VideoCapture cap(0);
    if (!cap.isOpened())
    {
        std::cout << "！！！";
        return -1;
    }
    Mat frame;
    bool judge = true;
    namedWindow("test1");
    while (judge)
    {
        cap >> frame;
        if (frame.empty())
            break;
        imshow("test1", frame);
        if (27 == waitKey(30))
        {
            break;
        }
    }
    destroyWindow("test1");
    return 0;
}
```



## 4、警告解决

这种警告一般可以忽略，出现这种错误的原因一般为勾选了某个库，但是并没有进行安装。

![image-20220902093314436](http://doc.xjfyt.top/markdown_img/image-20220902093314436.png)



代码中添加以下内容就不会出现上述信息了

```cpp
#include <opencv2/core/utils/logger.hpp>

utils::logging::setLogLevel(utils::logging::LOG_LEVEL_SILENT);　
```

