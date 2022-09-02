# Vue

## 1、创建一个vite项目

```bash
npm init vite@latest
```

### （1）选择

project->name->vue->vue-ts

### （2）安装依赖

切换到项目目录，执行

```bash
npm install 
```

### （3）启动项目

```bash
npm run dev
```

## 2、安装Vue cli脚手架

```bash
npm install @vue/cli -g
```

### （1）检测是否安装
```bash
vue -V
```

### （2）创建cli项目

```bash
vue create projectname
```

### （3）使用ui创建项目

```bash
vue ui
```

![image-20220831201405585](http://doc.xjfyt.top/markdown_img/image-20220831201405585.png)



### （4）启动项目

vue-cli创建的项目启动和使用npm创建的有所不同

**使用哪个启动项目要查看package.json中的scripts内容，如果是dev，则使用npm run dev，但如果是serve，则使用npm run serve**

```bash
npm run serve # 运行项目
npm run build # 打包项目
npm run lint  # 修复错误的配置
```



## 3、学习内容

```bash
vue基础
vue-cli 脚手架，命令行工具
vue-router 路由
vuex
element-ui
vue3
```

## 4、npm换源

### （1）、npm设置淘宝镜像

```bash
npm config set registry http://registry.npmmirror.com
```

### （2）、查看镜像设置

```bash
npm config get registry
```

### （3）、还原成npm镜像

```bash
npm config set registry https://registry.npmjs.org/
```

### （4）、清除npm缓存

```bash
npm cache clean --force
```



