# 解决npm警告问题



## 一、问题描述

```bash
npm WARN config global `--global`, `--local` are deprecated. Use `--location=global` instead.
```

好像是因为版本问题



## 二、解决

以管理员用户打开终端，输入：

```bash
npm install -g npm-windows-upgrade
```

若不可运行脚本，执行：

```bash
set-ExecutionPolicy RemoteSigned
```



然后更新

```bash
npm-windows-upgrade
```

选最新版本更新即可