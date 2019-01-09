---
layout:     post                    # 使用的布局（不需要改）
title:      Java 导入数据到Excel并提供文件下载接口              # 标题 
subtitle:   #副标题
date:       2018-04-13              # 时间
author:     Howie                      # 作者
header-img: img/SpringBoot-SSM.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - redis

---

> 最近需要在服务器上安装 redis，虽然只是一个小事情，但这个过程中也遇到了不少的问题，所以做一个总结，也希望能给到其他人一些帮助。
>
> 本文记录了 linux 系统和 windows 系统的 redis 安装
# Linux 系统
#### 1. 下载压缩文件
```
$ wget http://download.redis.io/releases/redis-4.0.9.tar.gz
```
可以更改版本号如： redis-4.0.0，redis-4.0.8
如果希望指定下载路径，就先 cd 进相应的目录中，否则默认 root 目录

#### 2. 解压安装
```
$ tar xzf redis-4.0.9.tar.gz
$ cd redis-4.0.9
$ make
```
这时，有些人可能会出现 Error
```
cd src && make all
make[1]: Entering directory `/root/redis-4.0.9/src'
    CC adlist.o
/bin/sh: cc: command not found
make[1]: *** [adlist.o] Error 127
make[1]: Leaving directory `/root/redis-4.0.9/src'
make: *** [all] Error 2
```
这是因为你的 linux 系统中没有安装 gcc 编译器，用 ``` yum install gcc ``` 命令安装一下gcc 即可

如果继续出现错误，如：
```
zmalloc.h:50:31: error: jemalloc/jemalloc.h: No such file or directory 
zmalloc.h:55:2: error: #error “Newer version of jemalloc required” 
make[1]: * [adlist.o] Error 1 
```
则将 ```make``` 命令改为 ```make MALLOC=libc```

#### 3. 启动
```
$ src/redis-server
```
![成功启动](https://upload-images.jianshu.io/upload_images/8807674-35f2ea527347cf21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4. 修改 redis 为后台运行
修改 redis.conf 文件将 daemonize 改成 yes (Redis 服务默认是前台运行) 

![](https://upload-images.jianshu.io/upload_images/8807674-cc32d2290551e1af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每次修改完 redis.conf 文件需要重启 redis 并读取最新的配置文件

关闭 redis 可以 ```[root@izwz952coo41my3g6a3v2iz src]# redis-cli``` 进入 redis 中使用 shutdown
```
127.0.0.1:6379> shutdown
```
也可以直接用 kill 命令关闭进程

然后重新启动
```
[root@izwz952coo41 src]# ./redis-server ../redis.conf
```

#### 5. 开放 IP 链接权限
进行到这里，如果想要远程连接 redis 的话，会发现无法连接，而这是因为 redis 默认只支持本地访问，所以我们需要开放 IP 链接权限  

  修改 redis.conf 文件将 bind 127.0.0.1 注释掉

![](https://upload-images.jianshu.io/upload_images/8807674-8fd09f6981aa6a2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注释前查看进程

![](https://upload-images.jianshu.io/upload_images/8807674-da4c0c2205109ecf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注释后查看进程

![](https://upload-images.jianshu.io/upload_images/8807674-4bfc2fdf5def59be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说明已经开放权限

#### 6. 防火墙问题
如果这时远程连接 redis 还出现无法连接的问题的话，很有可能是防火墙的问题（如果你打开了防火墙的话）

在linux下的防火墙中开放6379端口
```firewall-cmd --zone=public --add-port=6379/tcp --permanent ```
如果显示 ```success``` 说明开放成功

然后重启防火墙 
```systemctl restart firewalld```

#### 7. 认证密码
redis 默认密码为空，如果想要更改 redis 的密码，可以直接更改 redis.conf 文件中的 requirepass

![](https://upload-images.jianshu.io/upload_images/8807674-2382161fb398022e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

密码更改后，在 redis 中输入命令可能会出现认证问题
```
127.0.0.1:6379> (error) NOAUTH Authentication required. 
```
只需要输入 (password 为你的密码) 即可
```
127.0.0.1:6379> auth password
OK
```

# Windows 系统
redis 并不正式支持 windows，但是 github 上有 Microsoft 开放技术小组开发和维护的基于 windows 端口的 redis，但值得注意的是，这个项目只支持 64 位系统

链接： https://github.com/MicrosoftArchive/redis/releases

下载之后解压，打开 cmd，进入 redis 目录后使用命令 ```redis-server.exe redis.windows.conf``` ，启动 redis 服务，另外使用 ```redis-cli.exe ``` 进入客户端
