## Node.js中的环境变量

可以`Node.js`应用进程外设置特殊的环境变量，对于在应用外部配置特别有用。 比方说，云提供商想要更改你应用程序的监听的端口，
或者你想要在不进入代码的情况下开启详细的日志输出。

本教程将向你展示如何在`Node.js`中使用环境变量。

### 设置环境
当使用`Node.js`启动应用时，`shell`的环境会被注入到应用进程。可以通过`process.env.ENV_NAME`使用这些变量。

### 环境变量`NODE_ENV`

环境变量`NODE_ENV`通常是用来指定`Node.js`应用的运行环境（通常是`production`或`development`）。

例如，使用`express`时，将`NODE_ENV`设置为`production`可以将性能提高`3`倍，参考这个
[文档](https://expressjs.com/en/advanced/best-practice-performance.html#set-node_env-to-production)。它会开启：

- 缓存视图模板.
- 缓存扩展为`CSS`文件.
- 生成更简短的错误信息。

你可以在`ecosystem.config.js`文件定义不同的环境。

```javascript
module.exports = {
  apps: [{
    name: "app",
    script: "./app.js",
    env: {
      NODE_ENV: "development"
    },
    env_production: {
      NODE_ENV: "production",
    }
  }]
}
```
使用`pm2 start app --env production`命令启动你的应用，并在`production`模式下。

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。

