# 使用Browserify进行模块化开发

## 什么是模块（Module）

很多编程语言都支持模块化代码，如Ruby中的gems、Python中的eggs以及Java中的packages。JavaScript官方从未支持这个概念，直到ES2015介绍了`modules`。由于JavaScript很晚才提出这个概念，所以有很多社区解决方案已经被提出。最出名的就是Node.js modules。

## Node.js中的模块化系统的作用

1. 允许开发者按逻辑将程序划分为不同的模块并且只暴露必要的APIs。
2. 由于模块只暴露`export`的内容，所以不需要在一个立即调用的函数表达式中包装所有内容。（只需调用需要的函数）
3. 模块只在`import`了这个模块的地方才生效，所以不会污染全局命名空间，也就不会产生意外的命名冲突。

## 什么是Browserify

Browserify的官方介绍为"Browserify lets you require('modules') in the browser by bundling up all of your dependencies."Browserify是一个允许你在开发过程中使用与Node.js相同的方式去定义模块并把这些模块组装成一个单一的文件的工具。

Browserify的工作流程大致如下：

Browserify通过一个入口文件（entry point）分析该程序所需要的依赖，最终去构建一颗依赖树。然后Browserify在维持模块的作用域和命名空间的前提下把这些模块打包并生成一个单一的JavaScript文件（bundled JavaScript file）。

这个文件可以被包含到前端的一个网页中。这使得前端开发者能够写模块化的JS代码和利用NPM上面的包。

## Babelify

Babel可以将新版本的JS代码转换为ES5代码，但是Babel并没有提供一个模块化系统，所以如何打包模块化代码需要我们自己解决。ES2015的确为JavaScript定义了官方的模块化规范，但是babel没有提供模块加载器。所以我们应该怎样才能让babel将ES2015代码转换为Browserify支持的代码结构。

Browserify中有个概念叫做`transform`, transform允许代码在被Browserify操作之前进行转变。针对babel的transform被称之为`babelify`。当我们使用babelify时，每一个文件在被送往Browserify之前会被转换，这使得我们可以使用ES2015的modules特性。

[Browserify Transform列表](https://github.com/browserify/browserify/wiki/list-of-transforms)

## 使用Browserify步骤

### 1. 安装Browserify

```shell
npm install browserify --global
```

### 2.创建一个使用babelify的工程

* 创建工程结构如下：

  ```
  babelify-demo
  └── src
  |   ├── app.js
  |   └── index.js
  ├── .babelrc   
  ```

* 初始化NPM工程并安装依赖和插件

  ```shell
  cd babelify-demo
  npm init -y
  npm install babelify@8 --save-dev #如果使用babel 6应该使用babelify 8
  npm install babel-core --save-dev #babelify依赖于babel-core
  npm install babel-preset-es2015 --save-dev
  npm install babel-preset-stage-0 --save-dev
  npm install babel-plugin-transform-decorators-legacy --save-dev
  ```

### 3.配置.babelrc文件

```json
{
  "presets": ["es2015", "stage-0"],
  "plugins": ["transform-decorators-legacy"],
  "sourceMaps": "inline"
}
```

### 4.修改app.js文件

```js
const MyModule = {
    check() {
      console.log('Hey! modules are working!!');
    }
  }
  
export default MyModule;
```

### 5.修改index.js

```js
import MyModule from './app';

MyModule.check();
```

### 6.运行打包命令

```
browserify src/index.js --transform babelify --outfile dist/bundle.js --debug
```

### 7.运行bundle.js

```shell
node dist/bundle.js #使用Node执行js文件的方式
```

执行完控制台会输出

```
Hey! modules are working!!
```

### 8.在项目的根目录下创建index.html如下

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Babelify Demo</title>
    </head>
    <body>
        <h1>
            Hello, ES6!
        </h1>

        <script src="dist/bundle.js"></script>
    </body>
</html>
```

在浏览器打开index.html，然后打开控制台会看到Hey! modules are working!!