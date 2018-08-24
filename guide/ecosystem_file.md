## 生态系统文件

当在多个服务器上部署或使用多个`CLI`参数时，使用生态系统文件来替代命令行启动应用，将会更加方便。

生态系统文件的目的是收集应用所有的配置选项和环境变量。

### 创建模版
创建一个`ecosystem.config.js`模版：
```bash
pm2 init
```

这个命令会生成：
```javascript
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    env: {
      NODE_ENV: "development",
    },
    env_production: {
      NODE_ENV: "production",
    }
  }]
}
```

更多可用的配置选项，查看[生态系统文件参考]()

### 生态系统文件使用
#### 常规
在你的源代码工作目录中，使用下面的命令可以将所有应用程序添加到进程列表中：
```bash
pm2 start
```

这个命令会运行定义在`ecosystem.config.js`文件（运行`pm2 init`生成）中的所有应用。

你也可以从指定目录中加载生态系统文件：
```bash
pm2 start /path/to/ecosystem.config.js
```

#### 针对指定的应用
只针对生态系统文件中定义的指定应用：
```bash
pm2 start --only app
```
### 环境变量
可以为多个环境声明变量, 环境变量的`key`必须遵守这种格式`env_<environment-name>`。

例如，下面例子中的`app`进程会在两个环境中启动：`development`和`production`。

```bash
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    env: {
      NODE_ENV: "development",
    },
    env_production: {
      NODE_ENV: "production",
    }
  }]
}
```
要在特定环境下启动应用，请使用`--env`标志，如上面的例子：
```bash
pm2 start ecosystem.config.js                   # uses variables from `env`
pm2 start ecosystem.config.js --env production  # uses variables from `env_production`
```

### 不可变的环境
进程一旦添加到进程列表，那么这个进程的环境就不会再改变。使用：
- 当前环境
- 生态系统文件

因此，如果进程重启时与当前环境不同或有了的新生态系统文件，进程环境不会改变。

这是为了确保应用重启时能够保持一致。

#### 更新环境
那么，如果想要强制更新环境，使用`--update-env`参数：
```bash
# refresh the environment
pm2 restart ecosystem.config.js --update-env

# switch the environment
pm2 restart ecosystem.config.js --env production --update-env
```

### 下一步

**[进程管理](process_management.md)**