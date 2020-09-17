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
## 命令
- docker ps  -a 查看正在运行的doacker的镜像
- docker run IMAGE
- docker start/stop/restart
- docker rm IMAGE/id  服务必须是非运行状态
- docker kill 
- docker logs -f IMAGES 查看镜像的日志
- docker run --name dss-mysql -e MYSQL_ROOT_PASSWORD=123454 -p 28001:3306 -d mysql  运行

## docker Compose 
- 安装
```bash 
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose


```
## 遇到的问题

- docker: Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers).
See 'docker run --help'.
解决方法：docker默认的源为国外官方源，下载速度较慢，可改为国内，加速。修改或新增 /etc/docker/daemon.json。修改后重启docker服务才生效（'systemctl daemon-reload ' and 'systemctl restart docker'）
```json
{ "registry-mirrors": 
    [
    "https://pee6w651.mirror.aliyuncs.com",
    "https://registry.docker-cn.com",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
    ]
}
```
- 对外开放端口
```bash
/sbin/iptables -I INPUT -p tcp --dport 28001 -j ACCEPT
```

