## API


pm2也可以以编程方式使用，意味着你可以直接在代码中管理进程，创建进程，即使主脚本退出也能保持它们持续运行。

### 简单范例

这个例子展示了如何用一些配置属性来启动`app.js`。 传入`start`方法的配置`object`与`ecosystem.config.js`文件的配置相同：

```bash
npm install pm2 --save
```

```javascript
const pm2 = require('pm2')

pm2.connect(function(err) {
  if (err) {
    console.error(err)
    process.exit(2)
  }

  pm2.start({
    script: 'app.js',
  }, (err, apps) => {
    pm2.disconnect()
    if (err) { throw err }
  })
})
```

> 如果你的脚本不能自行退出，要确保调用`pm2.disconnect()`。


## 编程式API

**`pm2.connect(errback)` 或 `pm2.connect(noDaemonMode, errback)`**
* `noDaemonMode` - （默认值：`false`）如果第一个参数传递为`true`，那么`pm2`将不会作为守护进程运行，并在相关脚本退出时停止运行。
如果`pm2`已经在运行，你的脚本将链接到现有的守护进程，但是一旦你的进程退出就会停止运行。
* `errback(error)` - 在连接完成是或启动`pm2`守护程序进程时调用。

要么连接到正在运行的`pm2`守护进程（"God"），要么启动一个守护进程。 一旦启动，`pm2`进程将在脚本退出后继续运行。


**`pm2.disconnect()`**

断开与`pm2`守护进程的连接。


**`pm2.killDaemon(errback)`**

杀掉`pm2`守护进程（与`pm2 kill`相同）。注意，当守护进程被杀掉时，它的所有进程也会被杀掉。 另外要注意，即使杀掉守护进程，你仍然须明确断开它。


**`pm2.start(options, errback)`**
**`pm2.start(jsonConfigFile, errback)`**
**`pm2.start(script, errback)`**
**`pm2.start(script, options, errback)`**
**`pm2.start(script, jsonConfigFile, errback)`**

- `script` - 要运行的脚本路径。
- `jsonConfigFile` - `JSON`文件的路径，它可以包含与`options`参数相同的选项。
- `errback(err,proc)` - 当 `script`启动时调用`errback`。`proc`参数将是一个 [pm2 进程对象](https://github.com/soyuka/pm2-notify#templating)
- `options` - `Object` 具有下面的选项:

**`pm2.stop(process, errback)`**
**`pm2.restart(process, errback)`**
**`pm2.delete(process, errback)`**
**`pm2.reload(process, errback)`**

- `process`  - 既可以是 `pm2.start``options`中给出的`name`，也可以是进程`ID`，或者是字符串"all"，以表示应该重启所有脚本。
- `errback(err, proc)`


**`pm2.list(errback)`**

* `errback（err，processDescriptionList）` - `processDescriptionList`参数将包含`pm2.describe`中定义的`processDescription`对象列表。


**`pm2.describe(process,errback)`**

* `errback(err, processDescription)`
* `processDescription` - 包含有关过程信息的一个对象。 这包含以下属性：
  * `name` - 在原始`start`命令中给出的名称。
  * `pid` - 进程中的`pid`.
  * `pm_id` - `pm2 God`守护进程的`pid`
  * `monit` - 一个包含以下内容的对象：
    * `memory` - 进程正在使用的字节数。
    * `cpu` - 此时进程正在使用的CPU百分比。
  * `pm2_env` - 进程环境中的路径变量列表。 这些变量包括：
    * `pm_cwd` - 进程的工作目录。
    * `pm_out_log_path` - 标准输出流日志文件路径。
    * `pm_err_log_path` - 标准错误流日志文件路径。
    * `exec_interpreter` - 使用的解释器。
    * `pm_uptime` - 该进程的正常运行时间。
    * `unstable_restarts` - 该进程经历的不稳定的重启次数。
    * `restart_time`
    * `status` - “在线”，“正在停止”，“已停止”，“启动”，“错误”或“一次启动状态”
    * `instances` - 运行实例的数量。
    * `pm_exec_path` - 脚本在此进程中运行的路径。


**`pm2.dump(errback)`**

* `errback(err, result)`


**`pm2.startup(platform, errback)`**

* `errback(err, result)`


**`pm2.flush(process, errback)`**

* `errback(err, result)`


**`pm2.reloadLogs(errback)`** - *Rotates* 日志文件。 新的日志文件将包含更高的数字（默认格式为 `${process.name}-${out|err}-${number}.log`）。

* `errback(err, result)`


**`pm2.launchBus(errback)`** - 打开一个消息总线。

* `errback(err, bus)` - 此`bus`将是一个用于收听和发送事件的[Axon子发射](https://github.com/tj/axon#pubemitter--subemitter)对象。


**`pm2.sendSignalToProcessName(signal, process, errback)`**

* `errback(err, result)`

### 发送消息给进程

```javascript
// pm2-call.js:
pm2.connect(() => {
  pm2.sendDataToProcessId({
    type: 'process:msg',
    data: {
      some: 'data',
      hello: true
    },
    id: 0,
    topic: 'some topic'
  }, (err, res) => {
  })
})

pm2.launchBus((err, bus) => {
  pm2_bus.on('process:msg', (packet) => {
    packet.data.success.should.eql(true)
    packet.process.pm_id.should.eql(proc1.pm2_env.pm_id)
    done()
  })
})
```

```javascript
// pm2-app.js:
process.on('message', (packet) => {
  process.send({
    type: 'process:msg',
    data: {
     success: true
    }
 })
})
```
