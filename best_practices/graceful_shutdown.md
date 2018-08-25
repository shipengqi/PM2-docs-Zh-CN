## 优雅关机

### 优雅关机
一次优雅的关机，你的应用必须经过`5`个步骤：

- 收到`stop`的通知
- 请求负载均衡器不再接收请求
- 完成所有正在进行的请求
- 释放所有资源（数据库，队列 ...）
- 退出

比方说，下面是一个`express`应用：
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

const server = require('http').createServer(app)
server.listen(port, () => {
  console.log('Express server listening on port ' + server.address().port)
})
```

我们首先要拦截`SIGINT`信号（`pm2 stop`发出的信号）:
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

const server = require('http').createServer(app)
server.listen(port, () => {
  console.log('Express server listening on port ' + server.address().port)
})

process.on('SIGINT', () => {
  console.info('SIGINT signal received.')
})
```

然后，我们要求`HTTP`服务停止接收请求并完成正在进行的请求：
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

const server = require('http').createServer(app)
server.listen(port, () => {
  console.log('Express server listening on port ' + server.address().port)
})

process.on('SIGINT', () => {
  console.info('SIGINT signal received.')

  // Stops the server from accepting new connections and finishes existing connections.
  server.close(function(err) {
    if (err) {
      console.error(err)
      process.exit(1)
    }
  })
})
```

最后，我们关闭所有资源的连接：
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

const server = require('http').createServer(app)
server.listen(port, () => {
  console.log('Express server listening on port ' + server.address().port)
})

process.on('SIGINT', () => {
  console.info('SIGINT signal received.')

  // Stops the server from accepting new connections and finishes existing connections.
  server.close(function(err) {
    // if error, log and exit with error (1 code)
    if (err) {
      console.error(err)
      process.exit(1)
    }

    // close your database connection and exit with success (0 code)
    // for example with mongoose
    mongoose.connection.close(function () {
      console.log('Mongoose connection disconnected')
      process.exit(0)
    })
  })
})
```

### 超时终止
默认情况下，`PM2`在发送`SIGKILL`信号之前，如果应用进程没用自己退出，`PM2`等待`1600`毫秒后会主动杀掉应用进程。

你可以在`ecosystem.config.js`文件中修改这个超时时间的值，以毫秒为单位：

```javascript
module.exports = {
  apps: [{
    name: "app",
    script: "./app.js",
    kill_timeout: 1600,
  }]
}
```

### 在windows下的优雅关机
当进程终止信号不可用时。这种情况下，应用需要监听`shutdown`事件：
```javascript
process.on('message', (msg) => {
  if (msg == 'shutdown') {
    console.log('Closing all connections...')
    setTimeout(() => {
      console.log('Finished closing connections')
      process.exit(0)
    }, 1500)
  }
})
```

### 优雅启动
在处理`HTTP`请求之前，应用通常需要连接到数据库或其他资源。

你的应用需经过这`3`个步骤来避免错误：

- 打开数据库连接
- 开始请求一个端口
- 通知`PM2`应用已准备就绪

首先，在`ecosystem.config.js`文件配置在`pm2`中启用`ready`信号：
```javascript
module.exports = {
  apps : [{
    name: "api",
    script: "./api.js",
    wait_ready: true,
    listen_timeout: 3000,
  }],
}
```

> 默认情况下，在`3000ms`后，`PM2`会考虑准备好你的应用。 使用`listen_timeout`改变这个值。

让我们继续使用之前的`express`应用:
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

server.listen(port, () => {
  console.log('Express server listening on port ' + server.address().port)
})
```
首先，等待数据库连接准备就绪:
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

const server = require('http').createServer(app)
mongoose.connect('mongodb://mongosA:27501,mongosB:27501', (err) => {
  server.listen(port, () => {
    console.log('Express server listening on port ' + server.address().port)
  })
})
```
最后，使用`process.send('ready')`通知`PM2`应用已准备好：
```javascript
const app = express()
const port = process.env.port || 8000

app.get('/', (req, res) => { res.end('Hello world') })

const server = require('http').createServer(app)
mongoose.connect('mongodb://mongosA:27501,mongosB:27501', (err) => {
  server.listen(port, () => {
    console.log('Express server listening on port ' + server.address().port)
    process.send('ready')
  })
})
```

### 在集群模式下优雅启动

在集群模式下，有一个默认系统，可以在应用接受连接时设置每个集群。 这里还有一个默认的超时时间为`3000ms`，可以使用生态系统文件中的`listen_timeout`属性设置。

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。