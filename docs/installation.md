## 安装

### 安装 pm2

使用`yarn`安装：
```bash
yarn global add pm2
```

使用`yarn`安装：
```bash
yarn global add pm2
```

在`debian`系统下，使用安装脚本：
```bash
apt update && apt install sudo curl && curl -sL https://raw.githubusercontent.com/Unitech/pm2/master/packager/setup.deb.sh | sudo -E bash -
```

使用`docker`安装, 参考这个[文档](docs/docker.md)。


#### 使用 CLI 自动完成安装
```bash
pm2 completion install
```

#### 源地图支持

源地图文件如果存在，源地图文件默认是会被自动检测的 (`app.js.map` for `app.js`）。

> 什么是源地图文件？如果你正使用`Babel`，·Typescript·或其他的`Javascript`超集，您可能已经注意到堆栈跟踪没有意义，
错误不会指出正确的行数。源地图文件则可以用来解决这个问题。

### 更新
```bash
npm install pm2 -g && pm2 update
```

为了刷新`pm2`的守护进程，`pm2 update`是必须的。

### 下一步

### 下一步

<h1 align="center">
    <a href="ecosystem_file.html">
      生态系统文件
    </a>
</h1>