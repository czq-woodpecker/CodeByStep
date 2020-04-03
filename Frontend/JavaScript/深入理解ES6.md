# 深入理解ES6

## 一、块级作用域绑定

### 变量提升机制（Hoisting）

在函数作用域或全局作用域中通过关键字`var`声明的变量，无论在哪里声明，都会被当成`作用域顶部`声明的变量。

```js
function printValue(condition) {
  if (condition) {
    var value = "blue";
    console.log("value in if = " + value)
  } else {
    console.log("value in else = " + value) //此处可以访问变量value，其值为undefined
  }
  console.log("value not in if else = " + value) //此处可以访问变量value，其值为undefined
}
```

无论condition为何值，变量value都会被创建。因为在预编译街道，JavaScript引擎会将上面的代码修改成下面这样：

```js
function printValue(condition) {
  var value;
  if (condition) {
    value = "blue";
    console.log("value in if = " + value)
  } else {
    console.log("value in else = " + value) //此处可以访问变量value，其值为undefined
  }
  console.log("value not in if else = " + value) //此处可以访问变量value，其值为undefined
}
```

