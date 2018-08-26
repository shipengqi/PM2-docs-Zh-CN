## 生态系统文件参考

生态系统文件的目的是收集应用所有的配置选项和环境变量。

它是一个`javascript`文件，`exports`一个包含所有配置选项的`object`。这个`object`有两个属性：
- `apps`, `Array` 一组应用的配置
- `deploy`, `Object ` 部署配置选项

```javascript
module.exports = {
  apps: [{}, {}],
  deploy: {}
}
```
### 应用选项

属性`apps`是包含了一组`Object`的数组。

选项名称|描述|类型|默认
---|---|---|---
`script`|启动脚本的路径，必填字段|`String`|
`name`|进程列表中的进程名称|`String`|没有扩展名的脚本文件名（`app.js` 对应的名字是`app`）
`cwd`|当前工作目录以启动进程|`String`|当前`shell`环境的`CWD`
`args`|传递给脚本的参数|`Array`,`String`|
`interpreter`|解释器绝对路径|`String`|`node`
`node_args`|传递给解释器的参数|`Array`,`String`|
`output`|`stdout`的文件路径（输出的行会追加到文件中）|`String`|`~/.pm2/logs/<app_name>-out.log`
`error`|`stderr`的文件路径（输出的行会追加到文件中）|`String`|`~/.pm2/logs/<app_name>-error.err`
`log`|组合`stdout`和`stderr`的文件路径（输出的行会追加到文件中)|`Boolean`,`String`|`/dev/null`
`disable_logs`|禁用日志存储|`Boolean`|
`log_type`|指定日志输出类型，可能的值为：`json`|`String`|
`log_date_format`|日志时间戳的格式，采用`moment.js`格式（例如`YYYY-MM-DD HH：mm Z`）|`String`|
`env`|指定要注入的环境变量|`Object`,`String`|
`^env_\S*$`|指定使用`--env <env_name>`时要注入的环境变量|`Object`,`String`|
`max_memory_restart`|如果进程内存超出这个指定的最大内存，会重新启动应用（格式：`[0-9]` ，`K`是`KB`, `M`是`MB`, `G`是`GB`, 默认是`B`）|`String`,`Number`|
`pid_file`|已启动进程的`pid`的文件路径，有`pm2`写入|`String`|`~/.pm2/pids/app_name-id.pid`
`restart_delay`|在重启崩溃应用的延迟时间，单位毫秒|`Number`|
`source_map_support`|启用或禁用源映射支持|`Boolean`|`true`
`disable_source_map_support`|启用或禁用源映射支持|`Boolean`|
`wait_ready`|如果为`true`，进程会等待`process.send（'ready'）`的`ready`事件|`Boolean`|
`instances`|指定集群模式下启动的实例数|`Number`|`1`
`kill_timeout`|指定`PM2`杀掉进程的超时时间，单位毫秒。`PM2`在发送`SIGKILL`信号之前，如果应用进程没用自己退出，`PM2`等待{kill_timeout}毫秒后会主动杀掉应用进程|`Number`|`1600`
`listen_timeout`|单位毫秒，`PM2`会监听应用是否`ready`，如果{listen_timeout}后没有`ready`会强制重载，则强制重载|`Number`|
`cron_restart`|一个`cron`模式来重启你的应用|`String`|
`merge_logs`|在集群模式下，将每种类型的日志合并到一个文件中（而不是每个集群都有单独的日志文件）|`Boolean`|
`vizion`|启用或禁用版本元数据（`vizion`库）|`Boolean`|`true`
`autorestart`|进程失败后启用或禁用自重启|`Boolean`|`true`
`watch`|启用或禁用观察模式|`Boolean`,`Array`,`String`|
`ignore_watch`|要忽略的路径列表（正则表达式）|`Array`,`String`|
`watch_options`|用作`chokidar`选项的对象（请参阅`chokidar`文档）|`Object`|
`min_uptime`|考虑应用启动的最小正常运行时间（格式为`[0-9]`, `h`小时，`m`分钟，`s`秒，默认为`ms`）|`Number`,`String`|`1000`
`max_restarts`|最多重启次数|`Number`|`16`
`exec_mode`|设置执行模式，可能的值为：`fork|cluster`|`String`|`fork`
`force`|即使脚本已经运行，也强制将其启动|`Boolean`|
`append_env_to_name`|将环境名称附加到应用名称|`Boolean`|
`post_update`|在从`Keymetrics`仪表板执行提取/升级操作之后执行的命令列表|`Array`|
`trace`|启用或禁用事务跟踪|`Boolean`|
`disable_trace`|启用或禁用事务跟踪|`Boolean`|`true`
`increment_var`|指定环境变量的名称以注入每个群集的增量|`String`|
`instance_var`|重命名`NODE_APP_INSTANCE`环境变量|`String`|`NODE_APP_INSTANCE`
`pmx`|开启或禁用`pmx`包装|`Boolean`|`true`
`automation`|开启或禁用`pmx`包装|`Boolean`|`true`
`treekill`|只`kill`主进程，不分离子进程|`Boolean`|`true`
`port`|注入PORT环境变量的快捷方式|`Number`|
`uid`|设置用户`ID`|`String`|当前用户的`uid`
`gid`|设置群组`ID`|`String`|当前用户的`uid`

### 部署选项

属性`deploy`是一个`Object`，每个对象属性定义了一个环境的部署配置选项。

结构：
```javascript
module.exports = {
  apps: [{}, {}],
  deploy: {
    production: {},
    staging: {},
    development: {}
  }
}
```

环境的部署配置选项：

选项名称|描述|类型|默认
---|---|---|---
`key`|`SSH`密钥的路径|`String`|`$HOME/.ssh`
`user`|`SSH`用户|`String`|
`host`|`SSH`主机|`[String]`|
`ssh_options`|`SSH`选项，不包括命令行标志，查看`man ssh`|`String`，`[String]`|
`ref`|`GIT`的`remote/branch`|`String`|
`repo`|`GIT`的`remote`|`String`|
`path`|服务器中的路径|`String`|
`pre-setup`|`pre-setup`，可以指定一条命令，也可以指定一个本地主机上的脚本的路径|`String`|
`post-setup`|`post-setup`，可以指定一条命令，也可以指定一个本地主机上的脚本的路径|`String`|
`pre-deploy-local`|预部署|`String`|
`post-deploy`|部署后执行|`String`|
