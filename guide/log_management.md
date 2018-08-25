## 日志管理

`PM2`可以获取实时日志并存储到硬盘中。

日志格式化的方式，创建日志文件的方式：所有内容都可以自定义。

### 访问日志
#### 实时日志
```bash
# all apps logs
pm2 logs

# only app logs
pm2 logs app
```

#### 日志文件
默认情况下，日志文件被保存到`$HOME/.pm2/logs`。

使用下面的命令可以清空所有的应用日志：
```bash
pm2 flush
```

### 日志文件配置
自定义日志文件保存的位置：
```javascript
module.exports = {
  apps: [{
      name: 'app',
      script: 'app.js',
      output: './out.log',
      error: './error.log',
	    log: './combined.outerr.log',
    }]
}
```

- `output` 只保存标准输出到文件 (console.log)
- `error` 只保存错误输出到文件 (console.error)
- `log` 结合`output`和`error`, 默认是禁用的

#### 循环日志
如果你想将日志分割到多个文件，而不是使用一个大的日志文件，使用`logrotate`：
```bash
pm2 install pm2-logrotate
```

如何[配置循环日志](https://github.com/keymetrics/pm2-logrotate)。

### 合并日志
在群集模式下，每个群集都有自己的日志文件。 你可以使用合并选项收集所有的日志到一个文件中：
```javascript
module.exports = {
  apps: [{
      name: 'app',
      script: 'app.js',
      output: './out.log',
      error: './error.log',
      merge_logs: true,
    }]
}
```

> 日志仍然分为`output/error/log`。

### 禁用日志

将日志输出到 `/dev/null` 来禁用日志：
```javascript
module.exports = {
  apps: [{
      name: 'app',
      script: 'app.js',
      output: '/dev/null',
      error: '/dev/null',
    }]
}
```

### 日志格式化
#### JSON
输出`JSON`格式的日志：
```bash
Hello World!
```
变为：
```javascript
{
   "message": "Hello World!\n",
   "timestamp": "2017-02-06T14:51:38.896Z",
   "type": "out",
   "process_id": 0,
   "app_name": "app"
}
```

添加下面的配置选项到你的生态系统文件`ecosystem.config.js`中：
```javascript
"log_type": "json"
```

#### 时间戳格式
在日志输出中添加时间戳：
```bash
Hello World!
```

变为：
```bash
12-02-2018: Hello World!
```

添加下面的配置选项到你的生态系统文件`ecosystem.config.js`中：
```javascript
"log_date_format": "DD-MM-YYYY"
```

时间戳格式必须遵循[moment.js格式](https://momentjs.com/docs/#/parsing/string-format/)。

### 下一步

**[启动钩子](startup_hook.md)**