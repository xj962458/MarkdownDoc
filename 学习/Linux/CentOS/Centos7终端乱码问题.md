## 1、查看当前语言环境

```bash
 locale
```

```bash
LANG=
LC_CTYPE="POSIX"
LC_NUMERIC="POSIX"
LC_TIME="POSIX"
LC_COLLATE="POSIX"
LC_MONETARY="POSIX"
LC_MESSAGES="POSIX"
LC_PAPER="POSIX"
LC_NAME="POSIX"
LC_ADDRESS="POSIX"
LC_TELEPHONE="POSIX"
LC_MEASUREMENT="POSIX"
LC_IDENTIFICATION="POSIX"
LC_ALL=
```



## 2、安装两个包

```bash
yum update -y && yum install kde-l10n-Chinese glibc-common -y
```



## 3、配置

* 转化语言环境和字符集

```bash
localedef -c -f UTF-8 -i zh_CN zh_CN.utf8
```



* 修改环境变量

```bash
vim /etc/profile
```

追加内容

```bash
export LANG=zh_CN.utf8
```

使其立即生效

```bash
source /etc/profile
```

