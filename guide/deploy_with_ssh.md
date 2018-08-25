## 使用SSH快速部署

在大部分部署流程中，常规的基本流程是使用`SSH`连接到多个服务器，`git pull`最新版本的代码，然后`reload`应用进程。

`PM2`的部署工具目的是自动执行部署任务。

之需要设置一组远程主机，预先部署/部署后的命令行操作，便可以完成。

### 安装
#### SSH设置

确保你本地机器上有`ssh`公钥：
```bash
ssh-keygen -t rsa
ssh-copy-id node@myserver.com
```
#### 生态系统文件

首先要配置所有必要的信息到`ecosystem.config.js`：
```javascript
module.exports = {
  apps: [{
    name: "app",
    script: "app.js"
  }],
  deploy: {
    // "production" is the environment name
    production: {
      // SSH key path, default to $HOME/.ssh
      key: "/path/to/some.pem",
      // SSH user
      user: "ubuntu",
      // SSH host
      host: ["192.168.0.13"],
      // SSH options with no command-line flag, see 'man ssh'
      // can be either a single string or an array of strings
      ssh_options: "StrictHostKeyChecking=no",
      // GIT remote/branch
      ref: "origin/master",
      // GIT remote
      repo: "git@github.com:Username/repository.git",
      // path in the server
      path: "/var/www/my-repository",
      // Pre-setup command or path to a script on your local machine
      pre-setup: "apt-get install git ; ls -la",
      // Post-setup commands or path to a script on the host machine
      // eg: placing configurations in the shared dir etc
      post-setup: "ls -la",
      // pre-deploy action
      pre-deploy-local: "echo 'This is a local executed command'"
      // post-deploy action
      post-deploy: "npm install",
    },
  }
}
```

> 更多部署选项的信息，请查看[生态系统文件参考](../reference/ecosystem_file.md)

> 注意，远程路径必须为空，因为它会被`pm2 deploy`填充。

#### 设置
使用下面的命令进行第一次部署并填充远程路径：
```bash
pm2 deploy production setup
```

#### 部署
一些有用的命令：
```bash
# Setup deployment at remote location
pm2 deploy production setup

# Update remote version
pm2 deploy production update

# Revert to -1 deployment
pm2 deploy production revert 1

# execute a command on remote servers
pm2 deploy production exec "pm2 reload all"
```

### 部署配置选项
使用`pm2 deploy help`命令查看部署帮助信息：
```bash
pm2 deploy <configuration_file> <environment> <command>

  Commands:
    setup                run remote setup commands
    update               update deploy to the latest release
    revert [n]           revert to [n]th last deployment or 1
    curr[ent]            output current release commit
    prev[ious]           output previous release commit
    exec|run <cmd>       execute the given <cmd>
    list                 list previous deploy commits
    [ref]                deploy to [ref], the "ref" setting, or latest tag
```

### 强制部署
你可能会得到这个消息：
```bash
--> Deploying to dev environment
--> on host 192.168.1.XX

  push your changes before deploying

Deploy failed
```

这意味这你本地系统的改变还没有`push`到远程的`git`仓库。并且部署脚本通过`git pull`获取到的更新不会在你的本地服务器上。
如果你想不提交任何数据进行部署，添加`--force`选项：
```bash
pm2 deploy ecosystem.json production --force
```

### 注意事项

- 你可以使用`--force`选项跳过本地更改检测
- 验证你的远程服务器是否具有`git clone`权限
- 你可以根据要部署代码的环境，声明特定的环境变量。 例如，要为生产环境声明变量，添加`"env_production": {}`并声明变量。
- 你可以在`package.json`中嵌入`apps`和`deploy`分区

### SSH clone 错误
在大部分情况下，`clone`错误都是由于`pm2`没有正确的密钥来`clone`你的`git`仓库。你需要在每一步都验证密钥是否可用。

**第一步** 如果你确定你的密钥可用，先在目标服务器上尝试运行`git clone your_repo.git`。如果成功，转到下一步。如果失败，
确保你的密钥保存在服务器上，并且已经添加到你的`git`账户中。

**第二步** 默认情况下`ssh-copy-id`为`id_rsa`。 如果这不是正确的密钥：
```bash
ssh-copy-id -i path/to/my/key your_username@server.com
```

这个命令会将你的公钥添加到 `~/.ssh/authorized_keys`文件中。

**第三步** 如果你得到下面的错误信息：
```bash
--> Deploying to production environment
--> on host mysite.com
  ○ hook pre-setup
  ○ running setup
  ○ cloning git@github.com:user/repo.git
Cloning into '/var/www/app/source'...
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights and that the repository exists.

**Failed to clone**

Deploy failed
```

你可能会想创建一个`ssh`配置文件。这可以确保从指定的仓库`clone`并使用正确的`ssh key`。
看[这个例子](https://gist.github.com/Protosac/c3fb459b1a942f161f23556f61a67d66)：
```bash
# ~/.ssh/config
Host alias
    HostName myserver.com
    User username
    IdentityFile ~/.ssh/mykey
# Usage: `ssh alias`
# Alternative: `ssh -i ~/.ssh/mykey username@myserver.com`

Host deployment
    HostName github.com
    User username
    IdentityFile ~/.ssh/github_rsa
# Usage:
# git@deployment:username/anyrepo.git
# This is for cloning any repo that uses that IdentityFile. This is a good way to make sure that your remote cloning commands use the appropriate key
```
### Windows下注意事项
在`Windows`下运行部署脚本，你需要使用像`bash`这样的`unix shell`，所以我们建议安装[Git bash](https://git-scm.com/download/win)，
[Babun](http://babun.github.io/)或 [Cygwin](https://cygwin.com/install.html)。

### 贡献
这个工具是`PM2`的一个单独模块，你可以在[这里](https://github.com/Unitech/pm2-deploy)贡献。

### 下一步
感谢阅读本指南。
现在您可以查看[参考](../reference)以掌握`PM2`的所有功能。

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
