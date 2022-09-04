## 1、安装 epel-release

```bash
sudo yum install epel-release -y
```



## 2、添加第三方软件源

```bash
curl -o /etc/yum.repos.d/konimex-neofetch-epel-7.repo https://copr.fedorainfracloud.org/coprs/konimex/neofetch/repo/epel-7/konimex-neofetch-epel-7.repo
```



## 3、使用包管理器安装 neofetch

```bash
sudo yum install neofetch -y
```

