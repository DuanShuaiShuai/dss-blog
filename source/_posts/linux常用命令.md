---
title: Linux 常用命令总结
---
## 文档型 文件相关命令
- ls
共显示了七列信息，从左至右依次为：权限、文件数、归属用户、归属群组、文件大小、创建日期、文件名称
    - d 第一位表示文件类型
        + d 文件夹
        + \- 普通文件
        + l 链接
        + b 块设备文件
        + p 管道文件
        + c 字符设备文件
        + s 套接口文件
    - rwx 第2-4位表示这个文件的属主拥有的权限。r是读、w是写、x是执行 
    - r-x 第5-7位表示和这个文件属主所在同一个组的用户所具有的权限 
    - r-x 第8-10位表示其他用户所具有的权限

    常用的linux文件权限
    - 444 r--r--r--
    - 600 drw-------
    - 644 drw-r--r--
    - 666 drw-rw-rw-
    - 700 drwx------
    - 744 drwxr--r--
    - 755 drwxr-xr-x
    - 777 drwxrwxrwx

    根目录结构
    - home 个人目录
    - etc 软件相关配置文件
    - usr 系统可执行的一些文件
        - local 本地可执行的文件
        - sbin 超级管理员可执行的文件
    - sys 系统相关
    - var 存放的一些日志文件
- touch 创建文件
- vi text.txt  编辑打开文件
    - i  insert
    - esc 退出insert状态
    - :wq 退出保存
    - :q！退出不保存
- cat 查看文件
- echo
    - echo '12324' >> test.txt  追加内容至文件最后
    - echo '12324' > test.txt  覆盖文件内
- rm 删除
    - rm test.txt  删除文件
    - rm -r testdir/ 删除文件夹
    - rm -rf  test.txt 强制删除
```sh 

```

## 硬件型 磁盘/进程/服务/网络
- service sshd status  查看ssh服务
- systemctl status docker  查看docker服务
- systemctl start docker  开启docker服务
- service sshd restart/start 重启/start
- service sshd stop    关闭
- top #查看运行的进程
```bash
    # yum 包管理工具
    lsb_release -a  # 查看linux内核/硬件资源
    # Distributor ID: CentOS
    # Description:    CentOS Linux release 7.6.1810 (Core)
    # Release:        7.6.1810
    # Codename:       Core
    uname -a 
    # Linux VM_0_7_centos 3.10.0-1062.9.1.el7.x86_64 #1 SMP Fri Dec 6 15:49:49 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
    df -Th # 查看磁盘的使用情况
    cd / #根目录
```

## 功能型 压缩/解压/下载/远程
- wget  下载
    - wget url.tar.gz
- tar  解压缩
    - tar zxvf etcbak.tar.gz  解压一个tar
    - tar xvf etcbak.tar         解开一个tar
    - tar zxvf  xxxx.tar.gz  .tar.gz后缀结尾的
    - tar -xvf 用于解压   .tar后缀结尾的
        - z  代表以.gz结尾的文件
        - x  代表解压缩
        - v  代表显示解压缩的过程
        - f  必须跟上要处理的文件名
    - tar cvf etcbak.tar etc/  打包一个tar
    - tar zcvf etcbak.tar.gz etc/ 打包压缩一个 tar
        - c 压缩
- grep 查看进程
    - ps -ef | grep <name>
    - kill -9 <pid>

- ssh root@ip  远程连接
- netstat -anlp | grep sshd   查看ssh远程端口
- vi /etc/ssh/sshd_config

