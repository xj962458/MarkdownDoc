## 1、添加SSH公钥
不用按github官方生成密钥的方法进行操作，本机自带密钥即可
用户家目录下的.ssh文件夹内，复制id_rsa.pub中的内容
![20220902161039](https://doc.xjfyt.top/markdown_img/20220902161039.png)

打开github,选择`Setting`，找到`SSH and GPG keys`，点击`New SSH key`
![20220902161214](https://doc.xjfyt.top/markdown_img/20220902161214.png)

![20220902161618](https://doc.xjfyt.top/markdown_img/20220902161618.png)

## 2、Linux下权限问题
Linux下可能会出现这种情况，是因为权限问题
![20220902161719](https://doc.xjfyt.top/markdown_img/20220902161719.png)

解决方法
```bash
chmod -R 700 ~/.ssh
```

## 3、配置用户名和密码
只配置密钥是无法拉取推送仓库的，还需要配置用户名和邮箱
```bash
git config --global user.email 邮箱
git config --global user.name github用户名
```
然后就可以完成对github中git仓库的拉取、提交和推送等任务了

## 4、说明
当更换计算机时，不需要重新生成密钥，只需要将原计算机用户目录下的.ssh文件夹复制到新计算机上即可，而且这种方法不受计算机系统限制，Windows下的文件，放到Linux下也可以正常使用，但需要注意权限问题。最重要的是不要泄露自己的公钥和私钥，否则可能会造成仓库，甚至服务器的损坏。