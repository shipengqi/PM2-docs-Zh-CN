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
-n --name &lt;name&gt;|为脚本设置一个 &lt;name&gt;
-i --instances &lt;number&gt;|启动[number]实例（针对联网应用）（负载均衡）
--parallel &lt;number&gt;|并行操作数（用于重启/重载）
-l --log [path]|指定整个日志文件（包括错误和输出）
-o --output &lt;path&gt;|指定日志文件
-e --error &lt;path&gt;|指定错误日志文件
-p --pid &lt;pid&gt;|指定pid文件
-k --kill-timeout &lt;delay&gt;|在发送最终SIGKILL信号处理进程前延迟
--listen-timeout &lt;delay&gt;|应用重载时监听超时
--max-memory-restart &lt;memory&gt;|指定用于自重启的最大内存量（以八位字节为单位或使用100M等syntax语法）
--restart-delay &lt;delay&gt;|指定重启之间的延迟（以毫秒为单位）
--env &lt;environment_name&gt;|指定环境以获取特定的env变量（用于JSON声明）
--log-type &lt;type&gt;|指定日志输出样式（默认情况下为原始，json可选）
-x --execute-command|使用fork系统执行程序
--max-restarts [count]|只能重启脚本COUNT次
-u --user &lt;username&gt;|在生成启动脚本时定义用户
--uid &lt;uid&gt;|使用&lt;uid&gt;权限运行目标脚本
--gid &lt;gid&gt;|使用&lt;gid&gt;权限运行目标脚本
--cwd &lt;path&gt;|将目标脚本作为&lt;username&gt;运行
--hp &lt;home path&gt;|生成启动脚本时定义主路径
--wait-ip|重写systemd脚本以等待完整的互联网连接以启动pm2
--service-name &lt;name&gt;|生成启动脚本时定义服务名称
-c --cron &lt;cron_pattern&gt;|基于cron模式重启正在运行的进程
-w --write|在本地文件夹中写入配置
--interpreter &lt;interpreter&gt;|解释器pm2应该用于执行应用（bash，python ...）
--interpreter-args &lt;arguments&gt;|解释选项（--node-args的别名）
--log-date-format &lt;date format&gt;|为日志添加自定义前缀时间戳
--no-daemon|如果pm2守护程序不存在，则在前台运行pm2守护程序
-a --update-env|在重启/重载时更新环境(-a &lt;=&gt; apply)
--source-map-support|强制源地图支持
--only &lt;application-name&gt;|与json声明一起，允许只运行一个应用
--disable-source-map-support|强制源地图支持
--wait-ready|请求pm2等待您的应用中准备就绪的事件
--merge-logs|合并来自不同实例的日志，但保持错误并分离
--watch [paths]|观察应用文件夹的更改(default: )
--ignore-watch &lt;folders&#124;files&gt;|文件夹/文件被忽略观测，应该是一个特定的名称或正则表达式 - 例如 --ignore-watch="test node_modules "some scripts""
--node-args &lt;node_args&gt;|空间定界参数以群集模式传递给节点 - 例如 --node-args="--debug=7001 --trace-deprecation"
--no-color|跳过颜色代码
--no-vizion|在无vizion功能的情况下启动一个应用（版本控制）
--no-autorestart|在无自重启下启动一个应用
--no-treekill|只kill主进程，不分离子进程
--no-pmx|在无pmx下启动一个应用
--no-automation|在无pmx下启动一个应用
--trace|使用km启用事务跟踪
--disable-trace|使用km禁用事务跟踪
--attach|在启动/重启/停止/重载后附加日志记录
--sort &lt;field_name:sort&gt;|根据字段名称进行排序
--v8|启用v8数据收集
--event-loop-inspector|在pmx中启用事件循环检查器转储
--deep-monitoring|启用所有监控工具（相当于--v8 --event-loop-inspector --trace）
-h, --help|输出使用信息

## pm2命令

命令名称|描述
---|---
start [options] &lt;file&#124;json&#124;stdin&#124;app_name&#124;pm_id...&gt;|启动并守护应用
trigger &lt;proc_name&gt; &lt;action_name&gt; [params]|部署您的json
deploy &lt;file&#124;environment&gt;|部署您的json
startOrRestart &lt;json&gt;|启动或重启JSON文档
startOrReload &lt;json&gt;|启动或正常重载JSON文件
pid [app_name]|返回[app_name]的pid或全部
startOrGracefulReload &lt;json&gt;|启动或正常重载JSON文件
stop [options] &lt;id&#124;name&#124;all&#124;json&#124;stdin...&gt;|停止一个进程（想再次启动，执行pm2 restart &lt;app&gt;）
restart [options] &lt;id&#124;name&#124;all&#124;json&#124;stdin...&gt;|重启一个进程
scale &lt;app_name&gt; &lt;number&gt;|根据total_number参数在群集模式中放大/缩小进程
snapshot|快照PM2内存
profile &lt;command&gt;|配置文件CPU
reload &lt;name&#124;all&gt;|重载进程（请注意，它是作用于使用HTTP / HTTPS的应用）
gracefulReload &lt;name&#124;all&gt;|正常重载一个进程。 发送“关机”消息关闭所有连接。
id &lt;name&gt;|按名称获取进程ID
delete &lt;name&#124;id&#124;script&#124;all&#124;json&#124;stdin...&gt;|停止并从pm2进程列表中删除一个进程
sendSignal &lt;signal&gt; &lt;pm2_id&#124;name&gt;|发送一个系统信号给目标进程
ping|ping pm2守护进程 - 如果没有作用，它会启动它
updatePM2|用本地PM2更新内存PM2
update|(别称）使用本地PM2更新内存中的PM2
install&#124;module:install [options] [module&#124;git:/]|安装或更新模块（或一组模块）并永久运行
module:update &lt;module&#124;git:/&gt;|更新模块并永久运行
module:generate [app_name]|在当前文件夹中生成一个样本模块
uninstall&#124;module:uninstall &lt;module&gt;|停止并卸载模块
publish&#124;module:publish|发布您当前所在的模块
set [key] [value]|设置指定的配置&lt;key&gt; &lt;value&gt;
multiset &lt;value&gt;|多重集，例如"key1 val1 key2 val2
get [key]|获取&lt;key&gt;的值
conf [key] [value]|获取/设置模块配置值
config &lt;key&gt; [value]|获取/设置模块配置值
unset &lt;key&gt;|清除指定的配置 &lt;key&gt;
report|为https://github.com/Unitech/pm2/issues 提供一个完整的pm2报告
link&#124;interact [options] [secret] [public] [name]|将操作链接到 keymetrics.io  - 命令可以停止&#124;询问&#124;删除&#124;重启
unlink|将操作取消链接到 keymetrics.io - 命令可以停止&#124;询问&#124;删除&#124;重启
unmonitor [name]|不监控目标进程
monitor [name]|监控目标进程
open|在浏览器中打开仪表板
register|在keymetrics上创建一个帐户
login|登录keymetrics并链接当前的PM2
web|在0.0.0.0:9615上启动一个health API
dump&#124;save|转储所有进程以便稍后复活它们
send &lt;pm_id&gt; &lt;line&gt;|发送stdin到&lt;pm_id&gt;
attach &lt;pm_id&gt; [comman]|将标准输入/标准输出附加到由&lt;pm_id&gt;标识的应用
resurrect|反串行化以前被废弃的进程
unstartup [platform]|禁用并清除自启动 -  [platform]=systemd,upstart,launchd,rcd
startup [platform]|在启动时为pm2设置脚本 -  [platform]=systemd,upstart,launchd,rcd
logrotate|复制默认的logrotate配置
ecosystem&#124;init [mode]|生成一个进程配置文件。(mode = null or simple)
reset &lt;name&#124;id&#124;all&gt;|重置进程的计数器
describe &lt;id&gt;|描述进程ID的所有参数
desc &lt;id&gt;|(别称) 描述进程ID的所有参数
info &lt;id&gt;|(别称) 描述进程ID的所有参数
show &lt;id&gt;|(别称) 描述进程ID的所有参数
list&#124;ls|列出所有进程
l|(别称) 列出所有进程
ps|(别称) 列出所有进程
status|(别称) 列出所有进程
jlist|以JSON格式列出所有进程
prettylist|以prettified JSON输出json
monit|开展短期监测
imonit|启动legacy termcap监测
dashboard&#124;dash|启动带有监控和日志的仪表板
flush|刷新日志
reloadLogs|重载所有日志
logs [options] [id&#124;name]|流日志文件。 默认流所有日志
kill|杀死守护进程
pull &lt;name&gt; [commit_id]|更新给定应用的存储库
forward &lt;name&gt;|将存储库更新为给定应用的下一次提交
backward &lt;name&gt;|将存储库降级到给定应用的前一次提交
gc|强制PM2触发垃圾收集
deepUpdate|执行PM2的深层更新
serve&#124;expose [path] [port]|运用端口通过http服务一个目录

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
