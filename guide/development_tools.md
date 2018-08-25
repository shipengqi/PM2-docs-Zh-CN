## 开发工具

`PM2`附带两个开发工具，可以帮助开发：热重载模式和静态文件服务器。

### 热重载模式
监听当前目录，发现文件改动则自动重启。

配置`ecosystem.config.js`开启：
```bash
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    watch: true,
  }]
}
```

> 注意，热重载模式会导致硬重启，并且不会发送`SIGINT`信号。

### 配置监听选项
可以使用高级选项指定监听路径，和指定忽略的路径。

```javascript
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    watch: ".",
  }]
}
```

- `watch`可以是一个**字符串**或者是包含了的一组要监听的路径的**数组**。 设置为`true`时，表示监听当前目录。
- `ignore_watch`可以是一个**字符串**或者是包含了的一组忽略监听的路径的**数组**。 被依赖包[chokidar](https://github.com/paulmillr/chokidar#path-filtering)当做一个`glob`或正则表达式使用。
- `watch_options`是一个对象，作为[chokidar](https://github.com/paulmillr/chokidar#path-filtering)依赖包的选项。
（`PM2`的默认选项是持久的，`ingoreInitial`设置为`true`）

当使用`NFS`时，要按照此[chokidar issue](https://github.com/paulmillr/chokidar/issues/242)设置`usePolling: true`。

#### 使用CLI
可以使用`CLI`开启监听模式：
```bash
pm2 start app.js --watch
```

> 但是，注意，当使用`--watch`时开启监听模式时，必须使用`pm2 stop --watch <app_name>`来停止该进程，否则不会停止监听模式。

### HTTP服务提供静态文件
`PM2`可以通过`HTTP`提供静态文件（如前端应用程序）：
```bash
pm2 serve <path> <port>
```

`path`和`port`的默认值分别是 当前路径 和 `8080`，可以直接使用：
```bash
pm2 serve
```

在生态系统文件中配置：
```javascript
module.exports = {
  apps: [{
    name: "static-file",
    script: "serve",
    env: {
      PM2_SERVE_PATH: ".",
      PM2_SERVE_PORT: 8080,
    },
  }]
}
```

并使用它启动：
```bash
pm2 start ecosystem.config.js
```

> 这里所有的`PM2`配置选项都是可用的。

### 下一步

**[使用SSH快速部署](deploy_with_ssh.md)**