## 与Heroku一起使用PM2

本节指导你一步步将`PM2`与`Heroku`集成。

我们会用到`Git`和`Heroku CLI`。

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

> 学习更多关于[生态系统文件](../guide/ecosystem_file.md)。

我们建议在`Heroku`中使用集群模式，因为每个`dyno`有多核`CPU`。

> 学习更多关于[集群模式](../guide/load_balancing.md)。

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

### 使用`Heroku`部署
#### 创建`Heroku`账户
[注册`Heroku`账户](https://signup.heroku.com/)

#### 安装CLI
[安装说明](https://devcenter.heroku.com/articles/heroku-cli)

然后，运行`heroku login`将`CLI`连接到你的帐户。

#### 初始化`Heroku`应用

我们先创建一个空的`Heroku`应用，并且关联一个空的`Git`仓库。

在应用的根目录下运行：

```bash
heroku create

Creating app... done, ⬢ guarded-island-32432
https://guarded-island-32432.herokuapp.com/ | https://git.heroku.com/guarded-island-32432.git
```

现在你有了一个名为`heroku`的`git remote`。 如果你有`push`到这个远程仓库，你的代码会自动部署在给定的`URL`。

#### 在`Heroku`上部署
添加并提交你所有的改动，然后运行：
```bash
git push heroku master
Initializing repository, done.
updating 'refs/heads/master'
remote: Compressing source files... done.
remote: Building source:
...
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/aqueous-temple-78487.git
```

### 准备好了

这样就结束了，部署的最后一行会给你应用程序可用的`URL`。

### 下一步

完成[生态系统文件](../guide/ecosystem-file.md)配置

使用[PM2 Plus](https://pm2.io/doc/en/plus/integration/elastic-beanstalk/)在仪表板上监控你的应用

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
