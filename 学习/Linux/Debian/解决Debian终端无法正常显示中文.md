## 1、安装locales
```bash
apt install locales -y
```

## 2、添加配置
```bash
dpkg-reconfigure locales
```
选择`zh_CN.UTF-8 UTF-8`
![20220907074812](http://doc.xjfyt.top/markdown_img/20220907074812.png)

## 3、查看语言设置
```bash
locales
```
保证`LANG`为`zh_CN.UTF-8 UTF-8`
![20220907074925](http://doc.xjfyt.top/markdown_img/20220907074925.png)

若不是，可以添加环境变量
```bash
export LANG=zh_CN.UTF-8 UTF-8
```
追加到`/etc/profile`文件中，然后再使其生效
```bash
source /etc/profile
```

## 4、效果
设置完成后重新打开终端，设置成功

**原来的显示**
![20220907075255](http://doc.xjfyt.top/markdown_img/20220907075255.png)
**现在的显示**
![20220907075323](http://doc.xjfyt.top/markdown_img/20220907075323.png)