创建`*.yml`或`*.yaml`文件
```yaml
version:"3.1"
services:
	Name:
		restart:always
		image:ubuntu:latest
		container_name:my_ubuntu
		ports:
			-8080:8080
		environment:
			TZ:...
		volums:
			path:...
```


## 常用命令
### 1、构建启动
```bash
docker-compose up -d 文件名
```

### 2、进入构建的容器
```bash
docker-compose exec 容器名:ID bash
```

### 3、删除所有容器
```bash
docker-compose down
```

### 4、显示所有容器
```bash
docker-compose ps
```

### 5、重启所有
```bash
docker-compose restart <>
```

### 6、构建
```bash
docker-compose build 文件名
```