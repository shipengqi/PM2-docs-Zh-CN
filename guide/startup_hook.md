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

启动钩子会自动载入之前已经保存的进程列表。

使用下面的命令保存进程列表:
```bash
pm2 save
```

**如果删除了所有进程，然后重新启动系统（或使用`pm2`更新），`pm2`会恢复所有之前保存在`dump file`中的进程。这是为了防止空的`dump file`的`bug`。**

如果你要创建一个空的`dump`文件，应该使用：
```bash
pm2 cleardump
```

### 禁用启动系统
```bash
pm2 unstartup
```

### 用户权限
如果你希望启动钩子在另一个用户下执行，使用 ``-u <username>`和 `--hp <user_home>`选项：
```bash
pm2 startup ubuntu -u www --hp /home/ubuntu
```

### 更新启动钩子
使用下面的命令更新启动钩子:
```bash
pm2 unstartup
pm2 startup
```

### 兼容性
所有支持的`init`系统：
- systemd: Ubuntu >= 16, CentOS >= 7, Arch, Debian >= 7
- upstart: Ubuntu <= 14
- launchd: Darwin, MacOSx
- openrc: Gentoo Linux, Arch Linux
- rcd: FreeBSD
- systemv: Centos 6, Amazon Linux

你可以指定你要使用的平台：
```bash
pm2 [startup | unstartup] [platform]
```

平台可以是以下面列举中的任何一个：
`ubuntu, ubuntu14, ubuntu12, centos, centos6, arch, oracle, amazon, macos, darwin, freebsd, systemd, systemv, upstart, launchd, rcd, openrc`

### 底层

- `ubuntu`使用`updaterc.d`和脚本`lib/scripts/pm2-init.sh`
- `centos/redhat`使用`chkconfig`和脚本`lib/scripts/pm2-init-centos.sh`
- `gentoo`使用`rc-update`和脚本`lib/scripts/pm2`
- `systemd`使用`systemctl`和脚本`lib/scripts/pm2.service`
- `darwin`使用`launchd`来加载指定的`plist`以便在重启后恢复进程.

### 下一步

**[负载均衡（集群模式）](load_balancing.md)**