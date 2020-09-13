---
title: Docker入门
---
## Docker
 
### 主要特性
- 文件、资源、网络隔离
- 变更管理、日志记录
- 写时复制

### 安装 
- 如果安装过 先卸载旧版本
```bash
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
```
- 安装依赖  
```bash
    sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
```
- 设置仓库
```bash
 sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
- 安装docker
```bash
    sudo yum install docker-ce docker-ce-cli containerd.io
    docker -v 
    docker run hello-world 
```
- Docker 镜像加速
    对于使用 systemd 的系统，请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）：

```bash
{"registry-mirrors":["https://reg-mirror.qiniu.com/"]}
```
之后重新启动服务： 
```bash 
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```
