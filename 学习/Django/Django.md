### 1、django常用命令

#### （1）创建项目

```shell
django-admin startproject project_name # 创建项目
python ./manage.py startapp app_name # 创建app
```



#### （2）运行项目

```
python ./manage.py runserver 			# 以默认形式运行项目
python ./manage.py runserver port       # 指定端口
python ./manage.py runserver ip:port    # 指定端口和ip
```



#### （3）数据库相关

```shell
python ./manage.py makemigrations # 创建更改的文件
python ./manage.py migrate        # 将生成的py文件应用到数据库
python ./manage.py flush		  #清空数据库
```



#### （4）管理员

```bash
python ./manage.py createsuperuser  # 创建用户
python manage.py changepassword username  # 修改用户密码
```



#### （5）Shell

```bash
python ./manage.py shell    # 项目环境终端
python ./manage.py dbshell  # 数据库命令行
```



#### （6）更多命令

```bash
python ./manage.py
```



### 2、设置`Simpleui`

#### （1）安装

```bash
pip install django-simpleui
```

