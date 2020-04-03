# var/const/let

## 看代码，猜结果

### 代码1

```js
for (var i = 0; i < 5; i++) {
  setTimeout(function () {
    console.log(i);
  }, 1);
};
for (let n = 0; n < 5; n++) {
  setTimeout(function () {
    console.log(n);
  }, 1);
};
```

> 结果是：第一个循环输出的全是5，第二个循环输出的是0，1，2，3，4，5.

### 代码2

```js
if(true) {
  let foo = 'bar';
}
console.log(foo);
```

> 结果是控制台报错，原因是foo未定义

`let变量有块作用域，只能在声明它的块或者子块中被访问`

A block is the body of a statement or function, it is the area between { and }.