# JavaScript对象知识

## 一、对象基础

对象是一个包含相关数据和方法的集合（通常由一些变量和函数组成，我们称之为对象里的属性和方法）。

### 点表示法

我们可以使用点表示法（dot notation）来访问对象的属性和方法。这种表示方法与Java相同。示例如下：

```js
person.age
person.interests[1]
person.bio()
```

### 括号表示法

括号表示法（bracket notation）是另一种访问属性的方式。

我们用点表示法访问对象的属性如下：

```js
person.age
person.name.first
```

而使用括号表示法访问如下：

```js
person['age']
person['name']['first']
```

> 这看起来很想访问一个数组的元素，从根本上来说是一回事。这里是根据值的名字去选择元素，而不是根据索引。这也就是对象有时被称为`关联数组`(associative array)的原因。
>
> 对象做了字符串到值的映射，而数组做的是数字值（索引）到值的映射。

### 设置对象成员

我们可以通过以下方式设置对象成员的值：

```js
person.age = 45
person['name']['last'] = 'Jacky'
```

`注意：设置对象成员并不意味着只能更新已经存在的属性的值，我们可以直接设置不存在的成员，js将会帮我们创建新的成员。`这一点Java中是不允许的。

```js
person['eyes'] = 'hazel'
person.farewell = function() { alert("Bye everybody!") }
```

此外，`括号表示法一个有用的地方是可以让我们动态地设置成员的名字`。

比如，我们通过两个input框能够让用户在他们的数据里存储自己定义的值类型，可以通过以下代码来获取用户输入的数据字段名称和该字段的值：

```js
var dataName = 'height';
var dataValue = '1.75m';

person[dataName] = dataValue;
```

## 二、创建对象的方式

### 构建函数

JavaScript没有像许多面对对象的语言一样有用于创建class类的声明（ES6中的class其实是语法糖）。JavaScript用一种称为构建函数的特殊函数来定义对象和它们的特征。

不像经典的面对对象语言，从构建函数创建的新实例的特征并非全盘复制，而是通过一个叫原型链的参考链链接过去的。所以这并非真正的实例，严格的讲，JavaScript在对象间使用和其他语言的共享机制不同。

让我们看看JavaScript如何通过普通函数定义一个"人"：

```js
function createNewPerson(name) {
  var obj = {};
  obj.name = name;
  obj.greeting = function () {
    alert('Hi, I am ' + this.name + '.');
  }
  return obj;
}
```

现在可以通过调用这个函数创建一个新的ava的人：

```js
var ava = createNewPerson('ava');
console.log(ava.name);
ava.greeting();
```

上述代码运行良好，但有点冗长。如果我们知道如何创建一个对象，就没有必要创建一个新的空对象并且返回它。幸好JavaScript通过构建函数提供了一个便捷的方法如下：

```js
function Person(name) {
  this.name = name;
  this.greeting = function () {
    alert('Hi, I am ' + this.name + '.');
  }
}
```

`这个构建函数是JavaScript版本的类`。它只定义了对象的属性和方法，除了没有明确创建一个对象和返回任何值以外，它有了 我们期待的函数所拥有的全部功能。这里使用了this关键字，即无论是该对象（类）的哪个实例被这个构建函数创建，它的name属性就是传递到构建函数形参name的值。

> 一个构建函数通常是大写字母开头，这样便于区分构建函数和普通函数。

那如何调用构建函数创建新的实例呢？

```js
var person1 = new Person('Bob');
var person2 = new Person('Saga');
```

当新的对象被创建，变量person1与person2有效地包含以下值：

```js
{
  name: 'Bob',
  greeting : function() {
    alert('Hi! I\'m ' + this.name + '.');
  }
}

{
  name: 'Saga',
  greeting : function() {
    alert('Hi! I\'m ' + this.name + '.');
  }  
}
```

> 注意，当我们每次调用构造函数时，我们都会重新定义一遍greeting(),这不是个理想的方法。为了避免这样，我们可以在原型里定义函数。

### 直接声明一个对象

```js
const person = {}
```

```js
const person = {
  name: ['Bob', 'Smith'],
  age: 32,
  gender: 'male',
  interests: ['music', 'skiing'],
  bio: function() {
    alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
  },
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};
```

### 通过Object()构造函数

首先，我们可以通过使用Object()构造函数来创建一个新对象。一般对象都有构造函数，它创建了一个空的对象。

1. 通过Object()构造函数创建一个新的对象

   ```js
   var person1 = new Object();
   ```

2. 此时，在person1变量中存储了一个空对象。然后根据需要，使用点或者括号表示法向此对象添加属性和方法。

   ```js
   person1.name = 'Chris';
   person1['age'] = 38;
   person1.greeting = function() {
     alert('Hi! I\'m ' + this.name + '.');
   }
   ```

3. 还可以将对象文本传递给Object()构造函数作为参数，以便属性/方法填充它。请尝试以下操作：

   ```js
   var person1 = new Object({
     name : 'Chris',
     age : 38,
     greeting : function() {
       alert('Hi! I\'m ' + this.name + '.');
     }
   });
   ```

### 使用create()方法

Object中有个create()方法，它允许我们基于现有对象创建新的对象。

```js
var person2 = Object.create(person1);

console.log(person2.name);
person2.greeting();
```

## 三、原型链

### 定义

JavaScript常被描述为一种`基于原型的语言`———每个对象拥有一个`原型对象`,对象以其原型为模板、从原型继承方法和属性。原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为`原型链`（prototype chain），它解释了为何一个对象会拥有定义在其他对象的属性和方法。

准确地说，这些属性和方法定义在Object的构造器函数(constructor functions)之上的`prototype`属性上，而非对象实例本身。

在传统的 OOP 中，首先定义“类”，此后创建对象实例时，类中定义的所有属性和方法都被复制到实例中。在 JavaScript 中并不如此复制——而是在对象实例和它的构造器之间建立一个链接（它是__proto__属性，是从构造函数的`prototype`属性派生的），之后通过上溯原型链，在构造器中找到这些属性和方法。

> **注意:** 理解对象的原型（可以通过`Object.getPrototypeOf(obj)`或者已被弃用的`__proto__`属性获得）与构造函数的`prototype`属性之间的区别是很重要的。前者是每个实例上都有的属性，后者是构造函数的属性。也就是说，`Object.getPrototypeOf(new Foobar())`和`Foobar.prototype`指向着同一个对象。

### 使用原型

在JavaScript中，函数可以有属性。`每个函数都有一个特殊的属性叫作原型（prototype）。`我们可以访问函数的原型如下：

```js
function doSomething(){}
console.log( doSomething.prototype );

// It does not matter how you declare the function, a
//  function in javascript will always have a default
//  prototype property.
var doSomething = function(){}; 
console.log( doSomething.prototype );
```

doSomething函数有一个默认的原型属性，类似下面这样：

```js
{
    constructor: ƒ doSomething(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        valueOf: ƒ valueOf()
    }
}
```

现在我们添加一些属性到doSomething的原型上：

```js
function doSomething(){}
doSomething.prototype.foo = "bar";
console.log( doSomething.prototype );
```

结果类似下面：

```js
{
    foo: "bar",
    constructor: ƒ doSomething(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        valueOf: ƒ valueOf()
    }
}
```

然后，我们在这个doSomething的原型基础上，创建一个它的实例，并在实例上添加一个属性如下：

```js
function doSomething(){}
doSomething.prototype.foo = "bar"; // add a property onto the prototype
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; // add a property onto the object
console.log( doSomeInstancing );
```

结果大致如下：

```js
{
    prop: "some value",
    __proto__: {
        foo: "bar",
        constructor: ƒ doSomething(),
        __proto__: {
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
```

doSomeInstancing的`__prop__`属性就是doSomething.prototype。

当我们访问doSomeInstancing的一个属性，浏览器首先查找doSomeInstancing是否有这个属性。如果没有，那么浏览器会在doSomeInstancing的`__prop__`中查找这个属性（也就是doSomething.proptotype）。如果还是没有找到，那么浏览器会继续往上查找，也就是查找doSomeInstancing的`__prop__`的`__prop__`，直到找到这个属性或者把所有`__prop__`都找完还是没找到则返回undefined。

默认情况下，所有函数的原型属性的`__prop__`就是`window.Object.prototype`。

所以 doSomeInstancing 的 `__proto__` 的 `__proto__` (也就是 doSomething.prototype 的 `__proto__` (也就是 `Object.prototype`)) 会被查找是否有这个属性。`注意：doSomeInstancing没有prototype属性。`

```js
function doSomething(){}
doSomething.prototype.foo = "bar";
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value";
console.log("doSomeInstancing.prop:      " + doSomeInstancing.prop);
console.log("doSomeInstancing.foo:       " + doSomeInstancing.foo);
console.log("doSomething.prop:           " + doSomething.prop);
console.log("doSomething.foo:            " + doSomething.foo);
console.log("doSomething.prototype.prop: " + doSomething.prototype.prop);
console.log("doSomething.prototype.foo:  " + doSomething.prototype.foo);
```

结果：

```
doSomeInstancing.prop:      some value
doSomeInstancing.foo:       bar
doSomething.prop:           undefined
doSomething.foo:            undefined
doSomething.prototype.prop: undefined
doSomething.prototype.foo:  bar
```

