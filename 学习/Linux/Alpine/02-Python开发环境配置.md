## 1、安装
```bash
apk add --update --no-cache curl jq py3-configobj py3-pip py3-setuptools python3 python3-dev
```

## 2、直接拉取python镜像的alpinel版本
```bash
docker pull python:alpine
```

直接用镜像创建容器后安装python可能会导致很多库安装失败