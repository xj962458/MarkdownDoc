# CentOS安装`librosa`及问题解决

## 1、安装`librosa`

```bash
python -m pip install librosa -i https://pypi.tuna.tsinghua.edu.cn/simple
```

此处使用清华镜像源进行安装，在国内速度较快



## 2、问题

以上安装安装成功了，但是呢，一旦导入`librosa`，就会报错，错误如下：

![image-20220528184259488](http://doc.xjfyt.top/markdown_img/image-20220528184259488.png)

网上很多说法是降级，安装旧版本的`librosa`，我试过，还是会有新的错误。



## 3、解决

上述错误，提示缺少`sndfile`库，安上就好了：

```bash
sudo yum install libsndfile -y
```

![image-20220528184711510](http://doc.xjfyt.top/markdown_img/image-20220528184711510.png)

再导入就一切正常