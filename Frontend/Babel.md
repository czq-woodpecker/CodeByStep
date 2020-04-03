

## 创建一个NPM工程

```shell
mkdir project-name
cd project-name
npm init -y #-y tells NPM to init project without asking questions and using the defaluts
```

## 安装Bable命令行接口（当前最新版本为6.9.0）

```shell
npm install babel-cli --save-dev
```

此时，Bable默认不会做任何转化，我们必须在项目中安装plugins或者presets依赖，并在Babel的配置中指定它们。

## 配置Babel

Babel使用.babelrc作为配置文件，配置文件要求如下：

1. babelrc文件必须放在Project的根目录下
2. 文件内容必须是有效的JSON格式

例如，如果我们想要让Babel转译所有ES2015（ES6）特性，我们可以使用ES2015 preset。将配置文件.babelrc修改如下：

```json
{
  "presets": ["es2015"]
}
```

## 安装ES2015 preset依赖

```shell
npm install babel-preset-es2015 --save-dev
```

## 测试Babel转码功能

1. 新建src目录，并在src下新建index.js文件

2. 在index.js中添加以下代码：

   ```js
   let foo = "bar";
   ```

3. 转换js代码

   ```shell
   #just for demo
   cd .. 
   #transpile js code in src directory to dist directory(will create dist directory) 
   babel src -d dist 
   # -d <directory-name> output the transpiled code to a directory instead
   ```

4. 打开dist/index.js，会看到：

   ```js
   "use strict";
   
   var foo = "bar";
   ```

## Stage-X preset

TC39是一个推动JavaScript发展的委员会，由各个主流浏览器厂商的代表构成，目的是指定ECMAScript标准，并落地实现。

TC39将ECMAScript提案分为以下5个阶段：

1. Stage 0 - **设想**（Strawman）：只是一个想法
2. Stage 1 - **建议**（Proposal）：这是值得跟进的
3. Stage 2 - **草案** （Draft）：初始规范
4. Stage 3 - **候选**（Candidate）： 完成规范并在浏览器上初步实现
5. Stage 4 - **完成**（Finished）：将添加到下一个年度版本发布中

Babel针对这5个阶段实现了Stage-x preset，stage-x preset中的语法会随着被批准为JavaScript新版本的组成部分而进行相应的改变。这些提案可能会有变化，所以特别是处于stage-3之前的任何提案，请谨慎使用。Babel会尽量在每次TC39会议之后尽快更新stage-x的preset。

如果我们使用stage-2 preset，那么我们可以使用达到2、3、4 阶段的特性，但不能使用仅在0、1阶段的特性。

我们可以使用stage 0 preset去尝试所有js新特性。如使用下面的配置：

```json
{
  "presets": ["es2015", "stage-0"],
  "plugins": ["transform-decorators-legacy"],
  "sourceMaps": "inline"
}
```

`transform-decorators-legacy`: 用于转换装饰器（类似Java的注解）的插件，是Babel 7 stage-0 preset的默认插件之一，但这里用的是babel 6，所以需要手动引入。

`"sourceMaps": "inline"`：大多数js源码都要经过压缩才能投入生产环境，压缩的源码非常不适合debug，因此sourceMaps被设计于用来展示原始的js源码，方便debug。

## 安装stage-0 preset与transform-decorators-legacy

```shell
npm install babel-preset-stage-0 --save-dev
npm install transform-decorators-legacy --save-dev
```

## 设置Babel为npm脚本

为了使用babel更加容易，我们可以添加npm script设置，打开package.json，并添加如下设置：

```json
"scripts": {
    "babel": "babel src -d dist"
},
```

运行这个npm script：

```shell
npm run babel
```

