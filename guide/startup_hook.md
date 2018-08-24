## 启动钩子

启动钩子的目的是为了保存进程列表并在机器重启时恢复，包括意外重启。

每个操作系统都有一个指定的工具来处理启动钩子：`pm2`提供了一种简单的方式来生成和配置它。

### 安装
检测机器初始化系统是否可用，并且生成配置，使用：
```bash
pm2 startup
$ [PM2] You have to run this command as root. Execute the following command:
$ sudo su -c "env PATH=$PATH:/home/unitech/.nvm/versions/node/v4.3/bin pm2 startup <distribution> -u <user> --hp <home-path>
```

在`CLI`中复制并粘贴这个命令的输出以设置启动钩子。

**如果你使用的是`NVM`，这里的`pm2`路径会在更新`nodejs`时改变。 每次更新后都需要运行`startup`命令。**

> 可以使用`--service-name <name>`选项自定义服务名称[#3213](https://github.com/Unitech/pm2/pull/3213)

### 保存进程列表

### 下一步

**[负载均衡](load_balancing.md)**