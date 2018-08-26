## 与Docker一起使用PM2

准备好包含`PM2`的`Node.js Docker`镜像。

`pm2-runtime`的目标是包装应用到合适的`Node.js`生产环境中。它解决了当在容器内部运行`Node.js`应用时遇到的主要问题：

- 支持第二进程回退（什么鬼？）保证应用高可靠性
- 进程流控制
- 自动监控应用，使应用始终保持稳定和高性能
- 支持自动发掘与解析源地图

此外，`PM2`作为容器和应用程序的中间层带来了`PM2`强大的特性，如生态系统文件、自定义日志系统和`pm2`的其他特性。

### 准备应用

#### 可用的镜像标签

**镜像名称** | **操作系统* | **Docker文件**
---|---|---
keymetrics/pm2:`latest-alpine`|[Alpine](https://www.alpinelinux.org/about/)|[latest-alpine](tags/latest/alpine/Dockerfile)
keymetrics/pm2:`8-alpine`|[Alpine](https://www.alpinelinux.org/about/)|[8-alpine](tags/8/alpine/Dockerfile)
keymetrics/pm2:`6-alpine`|[Alpine](https://www.alpinelinux.org/about/)|[6-alpine](tags/6/alpine/Dockerfile)
keymetrics/pm2:`4-alpine`|[Alpine](https://www.alpinelinux.org/about/)|[4-alpine](tags/4/alpine/Dockerfile)
**镜像名称** | **操作系统** | **Docker文件**
keymetrics/pm2:`latest-stretch`|[Debian Stretch](https://wiki.debian.org/DebianStretch)|[latest-stretch](tags/latest/stretch/Dockerfile)
keymetrics/pm2:`8-stretch`|[Debian Stretch](https://wiki.debian.org/DebianStretch)|[8-stretch](tags/8/stretch/Dockerfile)
keymetrics/pm2:`6-stretch`|[Debian Stretch](https://wiki.debian.org/DebianStretch)|[6-stretch](tags/6/stretch/Dockerfile)
keymetrics/pm2:`4-stretch`|[Debian Stretch](https://wiki.debian.org/DebianStretch)|[4-stretch](tags/4/stretch/Dockerfile)
**镜像名称* | **操作系统** | **Docker文件**
keymetrics/pm2:`latest-jessie`|[Debian Jessie](https://wiki.debian.org/DebianJessie)|[latest-jessie](tags/latest/jessie/Dockerfile)
keymetrics/pm2:`8-jessie`|[Debian Jessie](https://wiki.debian.org/DebianJessie)|[8-jessie](tags/8/jessie/Dockerfile)
keymetrics/pm2:`6-jessie`|[Debian Jessie](https://wiki.debian.org/DebianJessie)|[6-jessie](tags/6/jessie/Dockerfile)
keymetrics/pm2:`4-jessie`|[Debian Jessie](https://wiki.debian.org/DebianJessie)|[4-jessie](tags/4/jessie/Dockerfile)
**镜像名称* | **操作系统** | **Docker文件**
keymetrics/pm2:`latest-slim`|[Debian Jessie](https://wiki.debian.org/DebianJessie) (minimal packages)|[latest-slim](tags/latest/slim/Dockerfile)
keymetrics/pm2:`8-slim`|[Debian Jessie](https://wiki.debian.org/DebianJessie) (minimal packages)|[8-slim](tags/8/slim/Dockerfile)
keymetrics/pm2:`6-slim`|[Debian Jessie](https://wiki.debian.org/DebianJessie) (minimal packages)|[6-slim](tags/6/slim/Dockerfile)
keymetrics/pm2:`4-slim`|[Debian Jessie](https://wiki.debian.org/DebianJessie) (minimal packages)|[4-slim](tags/4/slim/Dockerfile)
**镜像名称** | **操作系统** | **Docker文件**
keymetrics/pm2:`latest-wheezy`|[Debian Wheezy](https://wiki.debian.org/DebianWheezy)|[latest-wheezy](tags/latest/wheezy/Dockerfile)
keymetrics/pm2:`8-wheezy`|[Debian Wheezy](https://wiki.debian.org/DebianWheezy)|[8-wheezy](tags/8/wheezy/Dockerfile)
keymetrics/pm2:`6-wheezy`|[Debian Wheezy](https://wiki.debian.org/DebianWheezy)|[6-wheezy](tags/6/wheezy/Dockerfile)
keymetrics/pm2:`4-wheezy`|[Debian Wheezy](https://wiki.debian.org/DebianWheezy)|[4-wheezy](tags/4/wheezy/Dockerfile)

更多[镜像信息](https://github.com/nodejs/docker-node#image-variants)。

> 每次[NodeJS's Docker images](https://hub.docker.com/r/library/node/tags/)构建时会自动触发这些镜像的构建。
  每次`push`到[Docker PM2's GitHub repo](https://github.com/keymetrics/docker-pm2)主分支时，会自动触发这些镜像的构建。
  每次`push`到[PM2's GitHub repo](https://github.com/Unitech/pm2)主分支时，会自动触发这些镜像的构建。

#### 使用

我们假设你项目的目录结构是下面这样的：

```
`-- your-app-name/
    |-- src/
        `-- app.js
    |-- package.json
    |-- ecosystem.config.js    (we will create this in the following steps)
    `-- Dockerfile             (we will create this in the following steps)
```

### 配置`ecosystem.config.js`

生成`ecosystem.config.js`模板：

```bash
pm2 init
```

修改`ecosystem.config.js`：

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

[生态系统文件教程](/guide/ecosystem-file.md)。


#### 配置`Dockerfile`

创建`Dockerfile`文件：

```dockerfile
FROM keymetrics/pm2:latest-alpine

# Bundle APP files
COPY src src/
COPY package.json .
COPY ecosystem.config.js .

# Install app dependencies
ENV NPM_CONFIG_LOGLEVEL warn
RUN npm install --production

# Expose the listening port of your app
EXPOSE 8000

# Show current folder structure in logs
RUN ls -al -R

CMD [ "pm2-runtime", "start", "ecosystem.config.js" ]
```

#### 建立镜像

在你的项目下运行命令：

```bash
docker build -t your-app-name .
```

### 启动容器

```bash
docker run -p 80:8000 your-app-name
```

> `-p 80:8000`映射主机端口`80`到应用端口`8000`

#### pm2命令

`pm2`命令仍然可以在的容器中使用，运行`docker exec`命令进入容器：

```bash
# Monitoring CPU/Usage of each process
docker exec -it <container-id> pm2 monit
# Listing managed processes
docker exec -it <container-id> pm2 list
# Get more information about a process
docker exec -it <container-id> pm2 show
# 0sec downtime reload all applications
docker exec -it <container-id> pm2 reload all
```

### 暴露安全`endpoint`

```Dockerfile
CMD ["pm2-runtime", "ecosystem.config.js", "--web"]
```

`--web [port]`选项允许通过`JSON API`暴露所有重要信号（docker实例+应用）.

> 在`shell`中安装`pm2`之后，运行`pm2-runtime -h`获取所有可用选项。·


#### 准备好了

就是这样！容器做好部署准备。

### 下一步

完成[生态系统文件](../guide/ecosystem-file.md)配置

使用[PM2 Plus](https://pm2.io/doc/en/plus/integration/elastic-beanstalk/)在仪表板上监控你的应用

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
