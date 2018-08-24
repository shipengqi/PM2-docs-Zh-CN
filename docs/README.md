<h1 align="center">
  PM2 Runtime 中文文档
</h1>

<p align="center">
  <a href="https://pm2.io/doc/en/runtime/overview/">
    <img alt="PM2 Runtime" src="img/runtime-black.png" width="50%" height="">
  </a>
</p>

<p align="center">
  本文档图片均来自
  <a href="https://pm2.io/doc/en/runtime/overview/">
    官方文档
  </a>
  ，如有侵权，请联系我(`pooky.shipengqi@gmial.com`)删除。
</p>


## 概述
为什么要使用`PM2`? 看完这篇介绍，你会对使用`PM2`进行进程管理有更好的理解。

### 持久化

使用`PM2`启动你的应用，你的应用在崩溃或者机器重启时都可以自动重启。

只需要简单的运行一条命令：
```bash
pm2 start app.js
```

<h1 align="center">
    <a href="quick_start.html">
      快速开始
    </a>
</h1>

### 进程管理

<p align="center">
   <img alt="PM2 LS" src="img/pm2ls.png" width="70%" height="">
</p>

使用`PM2`启动的应用都是守护进程，而且方便管理。
用下面的命令可以查看进程列表：
```bash
pm2 ls
```

使用`pm2 start`和`pm2 delete`添加或删除列表中的进程。
使用`pm2 start`、`pm2 stop`、`pm2 restart`你所有的进程。


<h1 align="center">
    <a href="process_management.html">
      进程管理
    </a>
</h1>

### 日志管理

应用日志会被保存到服务器硬盘目录`~/.pm2/logs/`中。

查看应用的实时日志：
```bash
pm2 logs <app_name>
```

<h1 align="center">
    <a href="log_management.html">
      日志管理
    </a>
</h1>

### 零配置负载均衡
`PM2`可以使用同一个端口创建多个子进程来扩展您的应用。这样可以实现不停机重启你的应用。

开启集群模式：
```bash
pm2 start -i max
```

<h1 align="center">
    <a href="load_balancing.html">
      负载均衡
    </a>
</h1>


### 终端监控

<p align="center">
   <img alt="PM2 Monit" src="img/monit.png" width="80%" height="">
</p>

`PM2`可以在终端内监控你的应用，检查应用的健康状态（CPU使用率，内存占用，分钟内请求次数等等）
```bash
pm2 monit
```

### 使用SSH部署
自动化部署，避免在一个接一个地通过`ssh`连接服务器。
```bash
pm2 deploy
```

<h1 align="center">
    <a href="deploy_with_ssh.html">
      使用SSH部署
    </a>
</h1>

### 下一步

<h1 align="center">
    <a href="quick_start.html">
      快速开始
    </a>
</h1>

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。