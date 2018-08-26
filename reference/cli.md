## CLI

## pm2 Flags

Flag名称|描述
---|---
-V, --version|输出版本号
-v --version|获取版本
-s --silent|隐藏所有消息
-m --mini-list|显示没有格式的压缩列表
-f --force|强制执行
--disable-logs|不要记录日志
-n --name &lt;name&gt;|为脚本设置 &lt;name&gt;
-i --instances &lt;number&gt;|启动实例的数量`[number]`（针对联网应用）（负载均衡）
--parallel &lt;number&gt;|并行操作数量（用于重启/重载）
-l --log [path]|指定整个日志文件（包括`error`和`stdout`）
-o --output &lt;path&gt;|指定日志文件
-e --error &lt;path&gt;|指定错误日志文件
-p --pid &lt;pid&gt;|指定`pid`文件
-k --kill-timeout &lt;delay&gt;|在发送最终`SIGKILL`信号处理进程前延迟时间
--listen-timeout &lt;delay&gt;|应用重载时监听超时
--max-memory-restart &lt;memory&gt;|指定应用自重启的最大内存量（以字节为单位，使用`syntax`语法，如`100M`）
--restart-delay &lt;delay&gt;|指定两次重启之间的延迟时间（单位毫秒）
--env &lt;environment_name&gt;|指定环境以获取特定的`env`变量（用于`JSON`声明）
--log-type &lt;type&gt;|指定日志输出类型（默认情况下为`String`，`json`可选）
-x --execute-command|使用`fork`系统执行程序
--max-restarts [count]|重启脚本最大次数
-u --user &lt;username&gt;|指定生成启动脚本时的用户
--uid &lt;uid&gt;|使用&lt;uid&gt;权限运行脚本
--gid &lt;gid&gt;|使用&lt;gid&gt;权限运行脚本
--cwd &lt;path&gt;|将目标脚本作为&lt;username&gt;运行
--hp &lt;home path&gt;|定义成启动脚本时的`home`路径
--wait-ip|覆盖`systemd`脚本等待完整的互联网连接以启动`pm2`
--service-name &lt;name&gt;|定义生成启动脚本时的服务名称
-c --cron &lt;cron_pattern&gt;|基于`cron`模式重启正在运行的进程
-w --write|在本地文件夹中写入配置
--interpreter &lt;interpreter&gt;|指定`pm2`用于执行应用的解释器（bash，python ...）
--interpreter-args &lt;arguments&gt;|解释器参数（--node-args的别名）
--log-date-format &lt;date format&gt;|为日志添加自定义前缀时间戳
--no-daemon|如果`pm2`守护程序不存在，则在前台运行`pm2`守护程序
-a --update-env|在重启/重载时更新环境(-a &lt;=&gt; apply)
--source-map-support|强制源地图支持
--only &lt;application-name&gt;|与`json`声明一起，允许只运行一个应用
--disable-source-map-support|强制源地图支持
--wait-ready|请求`pm2`等待应用中的`ready`事件
--merge-logs|合并来自不同实例的日志，但保持错误并分离
--watch [paths]|监听应用文件夹的更改(`default: ""`)
--ignore-watch &lt;folders&#124;files&gt;|指定忽略监听的文件夹/文件，应该是一个特定的名称或正则表达式，例如 `--ignore-watch="test node_modules "some scripts"`
--node-args &lt;node_args&gt;|传递给解释器的参数，例如 `--node-args="--debug=7001 --trace-deprecation"`
--no-color|跳过颜色
--no-vizion|在无`vizion`功能的情况下启动一个应用（版本控制）
--no-autorestart|在无自重启下启动一个应用
--no-treekill|只`kill`主进程，不分离子进程
--no-pmx|在无`pmx`下启动一个应用
--no-automation|在无`pmx`下启动一个应用
--trace|使用`km`启用事务跟踪
--disable-trace|使用`km`禁用事务跟踪
--attach|在启动/重启/停止/重载后追加日志记录
--sort &lt;field_name:sort&gt;|根据字段名称进行排序
--v8|启用`v8`数据收集
--event-loop-inspector|在`pmx`中启用事件循环检查器转储
--deep-monitoring|启用所有监控工具（相当于`--v8 --event-loop-inspector --trace`）
-h, --help|输出帮助信息

## pm2命令

命令名称|描述
---|---
start [options] &lt;file&#124;json&#124;stdin&#124;app_name&#124;pm_id...&gt;|启动并守护应用
trigger &lt;proc_name&gt; &lt;action_name&gt; [params]|部署你的`json`
deploy &lt;file&#124;environment&gt;|部署你的`json`
startOrRestart &lt;json&gt;|启动或重启`JSON`文档
startOrReload &lt;json&gt;|启动或优雅重载`JSON`文件
pid [app_name]|返回指定`[app_name]`的`pid`或全部`pid`
startOrGracefulReload &lt;json&gt;|启动或正常重载`JSON`文件
stop [options] &lt;id&#124;name&#124;all&#124;json&#124;stdin...&gt;|停止一个进程（想再次启动，执行`pm2 restart app`）
restart [options] &lt;id&#124;name&#124;all&#124;json&#124;stdin...&gt;|重启一个进程
scale &lt;app_name&gt; &lt;number&gt;|根据`total_number`参数在群集模式中放大/缩小进程
snapshot|`PM2`内存快照
profile &lt;command&gt;|配置文件`CPU`
reload &lt;name&#124;all&gt;|重载进程（请注意，它是作用于使用`HTTP/HTTPS`的应用）
gracefulReload &lt;name&#124;all&gt;|正常重载一个进程。 发送“关机”消息关闭所有连接。
id &lt;name&gt;|按名称获取进程`ID`
delete &lt;name&#124;id&#124;script&#124;all&#124;json&#124;stdin...&gt;|停止并从`pm2`进程列表中删除一个进程
sendSignal &lt;signal&gt; &lt;pm2_id&#124;name&gt;|发送一个系统信号给目标进程
ping|`ping` `pm2`守护进程，如果没有作用，它会启动它
updatePM2|用本地PM2更新内存`PM2`
update|(别名）使用本地`PM2`更新内存中的`PM2`
install&#124;module:install [options] [module&#124;git:/]|安装或更新模块（或一组模块）并永久运行
module:update &lt;module&#124;git:/&gt;|更新模块并永久运行
module:generate [app_name]|在当前文件夹中生成一个样本模块
uninstall&#124;module:uninstall &lt;module&gt;|停止并卸载模块
publish&#124;module:publish|发布你当前所在的模块
set [key] [value]|设置指定的配置&lt;key&gt; &lt;value&gt;
multiset &lt;value&gt;|多重集，例如`key1 val1 key2 val2`
get [key]|获取&lt;key&gt;的值
conf [key] [value]|获取/设置模块配置值
config &lt;key&gt; [value]|获取/设置模块配置值
unset &lt;key&gt;|清除指定的配置 &lt;key&gt;
report|为`https://github.com/Unitech/pm2/issues`提供一个完整的`pm2`报告
link&#124;interact [options] [secret] [public] [name]|将操作链接到`keymetrics.io`，命令可以`stop|info|delete|restart`
unlink|将操作取消链接到`keymetrics.io`，命令可以`stop|info|delete|restart`
unmonitor [name]|不监控目标进程
monitor [name]|监控目标进程
open|在浏览器中打开仪表板
register|在`keymetrics`上创建一个帐户
login|登录`keymetrics`并链接当前的PM2
web|在`0.0.0.0:9615`上启动一个`health API`
dump&#124;save|转储所有进程以便之后可以恢复它们
send &lt;pm_id&gt; &lt;line&gt;|发送`stdin`到&lt;pm_id&gt;
attach &lt;pm_id&gt; [comman]|将标准输入/标准输出附加到由&lt;pm_id&gt;标识的应用
resurrect|恢复之前转储的所有进程
unstartup [platform]|禁用并清除自启动，`[platform]=systemd,upstart,launchd,rcd`
startup [platform]|为`pm2`启动设置脚本，`[platform]=systemd,upstart,launchd,rcd`
logrotate|复制默认的`logrotate`配置
ecosystem&#124;init [mode]|生成一个进程配置文件。(`mode = null or simple`)
reset &lt;name&#124;id&#124;all&gt;|重置进程的计数器
describe &lt;id&gt;|描述进程`ID`的所有参数
desc &lt;id&gt;|(别名) 描述进程`ID`的所有参数
info &lt;id&gt;|(别名) 描述进程`ID`的所有参数
show &lt;id&gt;|(别名) 描述进程`ID`的所有参数
list&#124;ls|列出所有进程
l|(别名) 列出所有进程
ps|(别名) 列出所有进程
status|(别名) 列出所有进程
jlist|以`JSON`格式列出所有进程
prettylist|以`prettified JSON`输出`json`
monit|开启暂时的监控
imonit|启动`legacy termcap`监测
dashboard&#124;dash|启动带有监控和日志的仪表板
flush|刷新日志
reloadLogs|重载所有日志
logs [options] [id&#124;name]|流化日志文件。 默认流化所有日志
kill|杀死守护进程
pull &lt;name&gt; [commit_id]|更新给定应用的仓库
forward &lt;name&gt;|更新仓库为给定应用的下一次提交
backward &lt;name&gt;|降级仓库为给定应用的上一次提交
gc|强制`PM2`触发垃圾收集
deepUpdate|执行`PM2`的深层更新
serve&#124;expose [path] [port]|指定提供静态文件目录的`http`端口

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
