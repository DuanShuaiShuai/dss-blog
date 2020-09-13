---
title: NVM
---

## 介绍
    开发多个项目的时候,发现相应依赖的node版本不一样。对于同时维护多个版本的node将是一件繁琐的事情（卸载、安装、配置path...循环的这样）。而nvm就是解决这样的问题而产生的，一个node版本的管理工具。它可以方便的在同一台设备上进行多个node版本之间的任意切换。由于作者是window系统，所以简单地介绍window下的操作。详情也可以查看[NVM官网](https://github.com/nvm-sh/nvm)

### nvm-windows
    由于nvm不支持window系统，所以才会有[nvm-windows](https://github.com/coreybutler/nvm-windows),[下载](https://github.com/coreybutler/nvm-windows/releases)并安装(安装成功后,建议重启),比较简单。

### 使用

``` bash
nvm list  # 查看当前的node版本
nvm install <version>  # 下载使用的node版本
nvm uninstall <version> # 卸载node版本
nvm use <version>  # 切换到要使用的node版本
nvm off # 关闭
nvm on #开启
```
