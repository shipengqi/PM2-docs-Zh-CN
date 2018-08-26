## 云供应商

你可能发现在某些情况下你无法使用`CLI`启动`Node.js`应用。

在这种情况下，必须将`PM2`作为依赖项添加，并且必须通过调用脚本来启动。


### 准备应用
#### 配置`ecosystem.config.js`
创建`ecosystem.config.js`：
```bash
pm2 init
```

修改`ecosystem.config.js`模版：
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

学习更多关于[生态系统文件](../guide/ecosystem_file.md)。

#### 安装PM2

安装`PM2`到项目依赖：
```bash
npm install --save pm2

# with yarn
yarn add pm2
```

#### 配置`package.json`
修改`package.json`中的`scripts`字段的`start`脚本：
```javascript
"scripts": {
  "start": "pm2-runtime start ecosystem.config.js --env production"
}
```

### 部署应用

现在，你可以像常规的`node.js`应用那样将应用部署到云供应商环境中。

### 下一步

完成[生态系统文件](../guide/ecosystem-file.md)配置

使用[PM2 Plus](https://pm2.io/doc/en/plus/integration/elastic-beanstalk/)在仪表板上监控你的应用

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
