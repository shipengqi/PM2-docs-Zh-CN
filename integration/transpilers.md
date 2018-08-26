## 与Transpiler一起使用PM2

本教程会展示如何与`Transpiler`一起使用`PM2`。

**我们强烈建议不要在`production`环境下中使用，实时转译会降低应用的运行速度。这种情况下，应用必须先从源代码转译，得到应用的预处理版本（bundle 文件）。**

### Typescript
```bash
## Add the new typescript dependency in PM2:
pm2 install typescript

## Start app.ts in watch & restart:
pm2 start app.ts --watch
```

### Babel
```bash
## Install the Babel CLI globally:
npm install -g babel-cli

## Start pm2 with the Babel CLI binary in watch mode:
pm2 start --watch --interpreter babel-cli app.js
```

或者，可以创建一个新的文件，在文件中`require`转译器和应用入口文件：
```javascript
// index.js
require('babel-register');
require('./app.js');
```
然后运行：
```bash
pm2 start --watch index.js
```

> 只有选择第二种方式可以使用群集模式。

### Coffee-script
```bash
## Install Coffee Script at PM2 level:
pm2 install coffeescript

## Start pm2 with coffee binary in watch mode:
pm2 start app.coffee
```

或者，可以运行**`npm install coffeescript`**，并且创建一个新文件，在文件中`require`转译器和应用入口文件：

```javascript
// index.js
require('coffee/register');
require('./app.coffee');
```

然后运行：
```bash
pm2 start --watch index.js
```

> 只有选择第二种方式可以使用群集模式。

### 问题
我们很乐于帮你解决你可能遇到的问题。搜索或查看`FAQ`。你也可以在`PM2`的[GitHub仓库](https://github.com/Unitech/pm2/issues)提交问题或评论。
