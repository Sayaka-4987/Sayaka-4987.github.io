---
layout:     post   				        # 使用的布局（不需要改）
title:      试图速成 JavaScript 和 React 	# 标题 
subtitle:   数据库老师的任务罢了		    # 副标题
date:       2021-10-24 				    # 时间
author:     WYX 					    # 作者
header-img: img/post-bg-js-version.jpg 	# 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - 施工中
    - JavaScript
---

# 全栈公开课 fullstackopen.com

https://fullstackopen.com/zh/#course-contents 

## 组件事件处理

必须始终通过将状态设置为新对象来更改状态，所以即使是是+1操作也必须建一个新变量，再把状态置为新值

```react
import React, { useState } from 'react'

const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)

  return (
    <div>
      {left}
      <button onClick={() => setLeft(left + 1)}>left</button>
      <button onClick={() => setRight(right + 1)}>right</button>
      {right}
    </div>
  )
}
```

组件也可以是任意类型

```react
const App = () => {
  const [clicks, setClicks] = useState({
    left: 0, right: 0
  })

  const handleLeftClick = () => {
    const newClicks = { 
      left: clicks.left + 1, 
      right: clicks.right 
    }
    setClicks(newClicks)
  }
 
  /* 换成展开语法： */
  const handleRightClick = () => {
  const newClicks = { 
    ...clicks, 	// 具有 clicks 对象的所有属性的副本
    right: clicks.right + 1  // 新对象中 right 属性的值将为 clicks.right+1
  }
  setClicks(newClicks)
}

  return (
    <div>
      {clicks.left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {clicks.right}
    </div>
  )
}
```

练习题

```react
import React, { useState } from 'react'

const Statistics = (props) => {
  let good = props.good
  let neutral = props.neutral
  let bad = props.bad
  if (good + neutral + bad === 0) {
    return <p>还没有评价！</p>
  }
  return [
    <table>
      <tr><td>good</td><td>{good}</td></tr>
      <tr><td>neutral</td><td>{neutral}</td></tr>
      <tr><td>bad</td><td>{bad}</td></tr>
      <tr><td>all</td><td>{good + neutral + bad}</td></tr>
      <tr><td>average</td><td>{1.0 * good - 1.0 * bad}</td></tr>
      <tr><td>positive</td><td>{1.0 * good / (good + neutral + bad)}</td></tr>
    </table>
  ]
}

const App = () => {
  // save clicks of each button to own state
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  return (
    <div>
      <h1>评价</h1>
      <button onClick={() => setGood(good + 1)}>good</button>
      <button onClick={() => setNeutral(neutral + 1)}>neutral</button>
      <button onClick={() => setBad(bad + 1)}>bad</button>
      <h1>统计</h1>
      <Statistics good={good} neutral={neutral} bad={bad} />
    </div>
  )
}

export default App
```



## Map 方法

index.js 内容如下:

```react
import ReactDOM from 'react-dom'
import App from './App'

const notes = [
  {
    id: 1,
    content: 'HTML is easy',
    date: '2019-05-30T17:30:31.098Z',
    important: true
  },
  {
    id: 2,
    content: 'Browser can execute only JavaScript',
    date: '2019-05-30T18:39:34.091Z',
    important: false
  },
  {
    id: 3,
    content: 'GET and POST are the most important methods of HTTP protocol',
    date: '2019-05-30T19:20:14.298Z',
    important: true
  }
]

ReactDOM.render(
  <App notes={notes} />,
  document.getElementById('root')
)
```

map 方法生成的每个元素必须有一个名为 key 的属性

```react
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>
            {note.content}
          </li>        
        )}
      </ul>
    </div>
  )
}
```



# X 分钟速成 Y，其中 Y=javascript

内容来自 https://learnxinyminutes.com/docs/zh-cn/javascript-cn/

用于写法速查；

```javascript
// 注释方式和C很像，这是单行注释
/* 这是多行
   注释 */

// 语句可以以分号结束
doStuff();

// ... 但是分号也可以省略，每当遇到一个新行时，分号会自动插入（除了一些特殊情况）。
doStuff()

// 因为这些特殊情况会导致意外的结果，所以我们在这里保留分号。
```



## 1. 数字、字符串与操作符

```javascript
// Javascript 只有一种数字类型(即 64位 IEEE 754 双精度浮点 double)。
// double 有 52 位表示尾数，足以精确存储大到 9✕10¹⁵ 的整数。
3; // = 3
1.5; // = 1.5

// 所有基本的算数运算都如你预期。
1 + 1; // = 2
0.1 + 0.2; // = 0.30000000000000004
8 - 1; // = 7
10 * 2; // = 20
35 / 5; // = 7

// 包括无法整除的除法。
5 / 2; // = 2.5

// 位运算也和其他语言一样；当你对浮点数进行位运算时，
// 浮点数会转换为*至多* 32 位的无符号整数。
1 << 2; // = 4

// 括号可以决定优先级。
(1 + 3) * 2; // = 8

// 有三种非数字的数字类型
Infinity; // 1/0 的结果
-Infinity; // -1/0 的结果
NaN; // 0/0 的结果

// 也有布尔值。
true;
false;

// 可以通过单引号或双引号来构造字符串。
'abc';
"Hello, world";

// 用！来取非
!true; // = false
!false; // = true

// 相等 ===
1 === 1; // = true
2 === 1; // = false

// 不等 !=
1 !== 1; // = false
2 !== 1; // = true

// 更多的比较操作符 
1 < 10; // = true
1 > 10; // = false
2 <= 2; // = true
2 >= 2; // = true

// 字符串用+连接
"Hello " + "world!"; // = "Hello world!"

// 字符串也可以用 < 、> 来比较
"a" < "b"; // = true

// 使用“==”比较时会进行类型转换...
"5" == 5; // = true
null == undefined; // = true

// ...除非你是用 ===
"5" === 5; // = false
null === undefined; // = false 

// ...但会导致奇怪的行为
13 + !0; // 14
"13" + !0; // '13true'

// 你可以用`charAt`来得到字符串中的字符
"This is a string".charAt(0);  // = 'T'

// ...或使用 `substring` 来获取更大的部分。
"Hello world".substring(0, 5); // = "Hello"

// `length` 是一个属性，所以不要使用 ().
"Hello".length; // = 5

// 还有两个特殊的值：`null`和`undefined`
null;      // 用来表示刻意设置的空值
undefined; // 用来表示还没有设置的值(尽管`undefined`自身实际是一个值)

// false, null, undefined, NaN, 0 和 "" 都是假的；其他的都视作逻辑真
// 注意 0 是逻辑假而  "0"是逻辑真，尽管 0 == "0"。
```



## 2. 变量、数组和对象


```javascript
// 变量需要用`var`关键字声明。Javascript是动态类型语言，
// 所以你无需指定类型。 赋值需要用 `=` 
var someVar = 5;

// 如果你在声明时没有加var关键字，你也不会得到错误...
someOtherVar = 10;

// ...但是此时这个变量就会在全局作用域被创建，而非你定义的当前作用域

// 没有被赋值的变量都会被设置为undefined
var someThirdVar; // = undefined

// 对变量进行数学运算有一些简写法：
someVar += 5; // 等价于 someVar = someVar + 5; someVar 现在是 10 
someVar *= 10; // 现在 someVar 是 100

// 自增和自减也有简写
someVar++; // someVar 是 101
someVar--; // 回到 100

// 数组是任意类型组成的有序列表
var myArray = ["Hello", 45, true];

// 数组的元素可以用方括号下标来访问。
// 数组的索引从0开始。
myArray[1]; // = 45

// 数组是可变的，并拥有变量 length。
myArray.push("World");
myArray.length; // = 4

// 在指定下标添加/修改
myArray[3] = "Hello";

// javascript中的对象相当于其他语言中的“字典”或“映射”：是键-值对的无序集合。
var myObj = {key1: "Hello", key2: "World"};

// 键是字符串，但如果键本身是合法的js标识符，则引号并非是必须的。
// 值可以是任意类型。
var myObj = {myKey: "myValue", "my other key": 4};

// 对象属性的访问可以通过下标
myObj["my other key"]; // = 4

// ... 或者也可以用 . ，如果属性是合法的标识符
myObj.myKey; // = "myValue"

// 对象是可变的；值也可以被更改或增加新的键
myObj.myThirdKey = true;

// 如果你想要获取一个还没有被定义的值，那么会返回undefined
myObj.myFourthKey; // = undefined

// 数组的 map 方法
const t = [1, 2, 3]
const m1 = t.map(value => value * 2)
console.log(m1)   // [2, 4, 6] is printed
```



#### 将字符串拆分为多行：回勾引号 ` 

```javascript
let str = `
  ECMA International's TC39 is a group of JavaScript developers,
  implementers, academics, and more, collaborating with the community
  to maintain and evolve the definition of JavaScript.
`;
```



## 3. 逻辑与控制结构

```javascript
// 本节介绍的语法与Java的语法几乎完全相同
// `if`语句和其他语言中一样。
var count = 1;
if (count == 3){
    // count 是 3 时执行
} else if (count == 4){
    // count 是 4 时执行
} else {
    // 其他情况下执行 
}

// while循环
while (true) {
    // 无限循环
}

// Do-while 和 While 循环很像 ，但前者会至少执行一次
var input;
do {
    input = getInput();
} while (!isValid(input))

// `for`循环和C、Java中的一样：
// 初始化; 继续执行的条件; 迭代。
for (var i = 0; i < 5; i++){
    // 遍历5次
}

// && 是逻辑与, || 是逻辑或
if (house.size == "big" && house.colour == "blue"){
    house.contains = "bear";
}
if (colour == "red" || colour == "blue"){
    // colour是red或者blue时执行
}

// && 和 || 是“短路”语句，它在设定初始化值时特别有用 
var name = otherName || "default";

// `switch`语句使用`===`检查相等性。
// 在每一个case结束时使用 'break'
// 否则其后的case语句也将被执行。 
grade = 'B';
switch (grade) {
  case 'A':
    console.log("Great job");
    break;
  case 'B':
    console.log("OK job");
    break;
  case 'C':
    console.log("You can do better");
    break;
  default:
    console.log("Oy vey");
    break;
}
```


## 4. 函数、作用域、闭包

```javascript
// JavaScript 函数由`function`关键字定义
function myFunction(thing){
    return thing.toUpperCase();
}
myFunction("foo"); // = "FOO"

// 注意被返回的值必须开始于`return`关键字的那一行，
// 否则由于自动的分号补齐，你将返回`undefined`。
// 在使用Allman风格的时候要注意.
function myFunction()
{
    return // <- 分号自动插在这里
    {
        thisIsAn: 'object literal'
    }
}
myFunction(); // = undefined

// javascript中函数是一等对象，所以函数也能够赋给一个变量，
// 并且被作为参数传递 —— 比如一个事件处理函数：
function myFunction(){
    // 这段代码将在5秒钟后被调用
}
setTimeout(myFunction, 5000);
// 注意：setTimeout不是js语言的一部分，而是由浏览器和Node.js提供的。

// 函数对象甚至不需要声明名称 —— 你可以直接把一个函数定义写到另一个函数的参数中
setTimeout(function(){
    // 这段代码将在5秒钟后被调用
}, 5000);

// JavaScript 有函数作用域；函数有其自己的作用域而其他的代码块则没有。
if (true){
    var i = 5;
}
i; // = 5 - 并非我们在其他语言中所期望得到的undefined

// 这就导致了人们经常使用的“立即执行匿名函数”的模式，
// 这样可以避免一些临时变量扩散到全局作用域去。
(function(){
    var temporary = 5;
    // 我们可以访问修改全局对象（"global object"）来访问全局作用域，
    // 在web浏览器中是`window`这个对象。 
    // 在其他环境如Node.js中这个对象的名字可能会不同。
    window.permanent = 10;
})();
temporary; // 抛出引用异常ReferenceError
permanent; // = 10

// javascript最强大的功能之一就是闭包。
// 如果一个函数在另一个函数中定义，那么这个内部函数就拥有外部函数的所有变量的访问权，
// 即使在外部函数结束之后。
function sayHelloInFiveSeconds(name){
    var prompt = "Hello, " + name + "!";
    // 内部函数默认是放在局部作用域的，
    // 就像是用`var`声明的。
    function inner(){
        alert(prompt);
    }
    setTimeout(inner, 5000);
    // setTimeout是异步的，所以 sayHelloInFiveSeconds 函数会立即退出，
    // 而 setTimeout 会在后面调用inner
    // 然而，由于inner是由sayHelloInFiveSeconds“闭合包含”的，
    // 所以inner在其最终被调用时仍然能够访问`prompt`变量。
}
sayHelloInFiveSeconds("Adam"); // 会在5秒后弹出 "Hello, Adam!"
```



## 5. 对象、构造函数与原型

```javascript
//  对象可以包含方法。
var myObj = {
    myFunc: function(){
        return "Hello world!";
    }
};
myObj.myFunc(); // = "Hello world!"

// 当对象中的函数被调用时，这个函数可以通过`this`关键字访问其依附的这个对象。
myObj = {
    myString: "Hello world!",
    myFunc: function(){
        return this.myString;
    }
};
myObj.myFunc(); // = "Hello world!"

// 但这个函数访问的其实是其运行时环境，而非定义时环境，即取决于函数是如何调用的。
// 所以如果函数被调用时不在这个对象的上下文中，就不会运行成功了。
var myFunc = myObj.myFunc;
myFunc(); // = undefined

// 相应的，一个函数也可以被指定为一个对象的方法，并且可以通过`this`访问
// 这个对象的成员，即使在函数被定义时并没有依附在对象上。
var myOtherFunc = function(){
    return this.myString.toUpperCase();
}
myObj.myOtherFunc = myOtherFunc;
myObj.myOtherFunc(); // = "HELLO WORLD!"

// 当我们通过`call`或者`apply`调用函数的时候，也可以为其指定一个执行上下文。
var anotherFunc = function(s){
    return this.myString + s;
}
anotherFunc.call(myObj, " And Hello Moon!"); // = "Hello World! And Hello Moon!"

// `apply`函数几乎完全一样，只是要求一个array来传递参数列表。
anotherFunc.apply(myObj, [" And Hello Sun!"]); // = "Hello World! And Hello Sun!"

// 当一个函数接受一系列参数，而你想传入一个array时特别有用。
Math.min(42, 6, 27); // = 6
Math.min([42, 6, 27]); // = NaN (uh-oh!)
Math.min.apply(Math, [42, 6, 27]); // = 6

// 但是`call`和`apply`只是临时的。如果我们希望函数附着在对象上，可以使用`bind`。
var boundFunc = anotherFunc.bind(myObj);
boundFunc(" And Hello Saturn!"); // = "Hello World! And Hello Saturn!"

// `bind` 也可以用来部分应用一个函数（柯里化）。
var product = function(a, b){ return a * b; }
var doubler = product.bind(this, 2);
doubler(8); // = 16

// 当你通过`new`关键字调用一个函数时，就会创建一个对象，
// 而且可以通过this关键字访问该函数。
// 设计为这样调用的函数就叫做构造函数。
var MyConstructor = function(){
    this.myNumber = 5;
}
myNewObj = new MyConstructor(); // = {myNumber: 5}
myNewObj.myNumber; // = 5

// 每一个js对象都有一个‘原型’。当你要访问一个实际对象中没有定义的一个属性时，
// 解释器就回去找这个对象的原型。

// 一些JS实现会让你通过`__proto__`属性访问一个对象的原型。
// 这虽然对理解原型很有用，但是它并不是标准的一部分；
// 我们后面会介绍使用原型的标准方式。
var myObj = {
    myString: "Hello world!"
};
var myPrototype = {
    meaningOfLife: 42,
    myFunc: function(){
        return this.myString.toLowerCase()
    }
};

myObj.__proto__ = myPrototype;
myObj.meaningOfLife; // = 42

// 函数也可以工作。
myObj.myFunc() // = "hello world!"

// 当然，如果你要访问的成员在原型当中也没有定义的话，解释器就会去找原型的原型，以此类推。
myPrototype.__proto__ = {
    myBoolean: true
};
myObj.myBoolean; // = true

// 这其中并没有对象的拷贝；每个对象实际上是持有原型对象的引用。
// 这意味着当我们改变对象的原型时，会影响到其他以这个原型为原型的对象。
myPrototype.meaningOfLife = 43;
myObj.meaningOfLife; // = 43

// 我们知道 `__proto__` 并非标准规定，实际上也没有标准办法来修改一个已存在对象的原型。
// 然而，我们有两种方式为指定原型创建一个新的对象。

// 第一种方式是 Object.create，这个方法是在最近才被添加到Js中的，
// 因此并不是所有的JS实现都有这个方法
var myObj = Object.create(myPrototype);
myObj.meaningOfLife; // = 43

// 第二种方式可以在任意版本中使用，不过必须通过构造函数。
// 构造函数有一个属性prototype。但是它 *不是* 构造函数本身的原型；相反，
// 是通过构造函数和new关键字创建的新对象的原型。
MyConstructor.prototype = {
    myNumber: 5,
    getMyNumber: function(){
        return this.myNumber;
    }
};
var myNewObj2 = new MyConstructor();
myNewObj2.getMyNumber(); // = 5
myNewObj2.myNumber = 6
myNewObj2.getMyNumber(); // = 6

// 字符串和数字等内置类型也有通过构造函数来创建的包装类型
var myNumber = 12;
var myNumberObj = new Number(12);
myNumber == myNumberObj; // = true

// 但是它们并非严格等价
typeof myNumber; // = 'number'
typeof myNumberObj; // = 'object'
myNumber === myNumberObj; // = false
if (0){
    // 这段代码不会执行，因为0代表假
}

// 不过，包装类型和内置类型共享一个原型，
// 所以你实际可以给内置类型也增加一些功能，例如对string：
String.prototype.firstCharacter = function(){
    return this.charAt(0);
}
"abc".firstCharacter(); // = "a"

// 这个技巧经常用在“代码填充”中，来为老版本的javascript子集增加新版本js的特性，
// 这样就可以在老的浏览器中使用新功能了。

// 比如，我们知道Object.create并没有在所有的版本中都实现，
// 但是我们仍然可以通过“代码填充”来实现兼容：
if (Object.create === undefined){ // 如果存在则不覆盖
    Object.create = function(proto){
        // 用正确的原型来创建一个临时构造函数
        var Constructor = function(){};
        Constructor.prototype = proto;
        // 之后用它来创建一个新的对象
        return new Constructor();
    }
}
```





# 现代 JavaScript 教程

内容来自 https://zh.javascript.info/



## 1. 简历、手册与规范

现代的 JavaScript 是一种“安全的”编程语言。它不提供对内存或 CPU 的底层访问，因为它最初是为浏览器创建的，不需要这些功能。

JavaScript 的能力很大程度上取决于它运行的环境。例如，[Node.js](https://wikipedia.org/wiki/Node.js) 支持允许 JavaScript 读取/写入任意文件，执行网络请求等的函数。

浏览器中的 JavaScript 可以做下面这些事：

- 在网页中添加新的 HTML，修改网页已有内容和网页的样式。
- 响应用户的行为，响应鼠标的点击，指针的移动，按键的按动。
- 向远程服务器发送网络请求，下载和上传文件（所谓的 [AJAX](https://en.wikipedia.org/wiki/Ajax_(programming)) 和 [COMET](https://en.wikipedia.org/wiki/Comet_(programming)) 技术）。
- 获取或设置 cookie，向访问者提出问题或发送消息。
- 记住客户端的数据（“本地存储”）。



JavaScript 是将这三件事结合在一起的唯一的浏览器技术:

- 与 HTML/CSS 完全集成。
- 简单的事，简单地完成。
- 被所有的主流浏览器支持，并且默认开启。



### 手册和兼容性表

[MDN（Mozilla）JavaScript 索引](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) 

https://caniuse.com/



### 按下 `F12` 开启开发者模式

开发者工具允许我们查看错误、执行命令、检查变量等

单击行号可以设置断点，**右键单击** 可以设置一个 **条件** 断点

也可以在代码中使用 `debugger` 命令来暂停代码

```javascript
function hello(name) {
  let phrase = `Hello, ${name}!`;

  debugger;  // <-- 调试器会在这停止

  say(phrase);
}
```



#### 输出到控制台：`console.log` 函数

```javascript
for (let i = 0; i < 5; i++) {
  console.log("value", i);
}
```



## 2.基础知识



### lambda 表达式

```javascript
let age = prompt("What is your age?", 18);

let welcome = (age < 18) ?
  () => alert('Hello') :
  () => alert("Greetings!");

welcome();
```



### 交互：alert、prompt 和 confirm

参考 https://developer.mozilla.org/zh-CN/docs/Web/API/Window

我们使用浏览器作为工作环境，所以基本的 UI 功能将是：

- `prompt(question[, default\])`

  提出一个问题，并返回访问者输入的内容，如果他按下「取消」则返回 `null` 

- `confirm(question)`

  提出一个问题，并建议用户在“确定”和“取消”之间进行选择。选择结果以 `true/false` 形式返回 

- `alert(message)`

  输出一个 `消息` 



这些函数都会产生 **模态框**，它们会暂停代码执行并阻止访问者与页面的其他部分进行交互，直到用户做出回答为止，上述所有方法共有两个限制：

1. 模态窗口的确切位置由浏览器决定，通常在页面中心
2. 窗口的确切外观也取决于浏览器，我们不能修改它

这就是简单的代价，还有其他一些方法可以显示更漂亮的窗口，并与用户进行更丰富的交互。



## 3.对象

JavaScript 中有八种基本的数据类型（译注：前七种为基本数据类型，也称为原始类型，而 `object` 为复杂数据类型）

- `number` 用于任何类型的数字：整数或浮点数，在 `±(253-1)` 范围内的整数
- `bigint` 用于任意长度的整数
- `string` 用于字符串：一个字符串可以包含 0 个或多个字符，所以没有单独的单字符类型
- `boolean` 用于 `true` 和 `false`
- `null` 用于未知的值 —— 只有一个 `null` 值的独立类型
- `undefined` 用于未定义的值 —— 只有一个 `undefined` 值的独立类型
- `symbol` 用于唯一的标识符



可以通过 `typeof` 运算符查看存储在变量中的数据类型。

- 两种形式：`typeof x` 或者 `typeof(x)`
- 以字符串的形式返回类型名称，例如 `"string"`
- `typeof null` 会返回 `"object"` —— 这是 JavaScript 编程语言的一个错误，实际上它并不是一个 `object`



`object` 对象的特点：

- `object` 用于更复杂的数据结构;
- `object` 对象通过引用被赋值和拷贝。
- 一个变量存储的不是“对象的值”，而是一个对值的“引用”（内存地址），因此，拷贝此类变量或将其作为函数参数传递时，所拷贝的是引用，而不是对象本身。
- 所有通过被拷贝的引用的操作（如添加、删除属性）都作用在同一个对象上。



### 创建对象

```javascript
let user = {     // 创建一个对象
  name: "John",  // 键 "name"，值 "John"
  age: 30        // 键 "age"，值 30
};

// 读取文件的属性：
alert( user.name ); // John
alert( user.age ); // 30
```



可以用多字词语来作为属性名，但必须给它们加上引号：

```javascript
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // 多词属性名必须加引号
};

let key = "likes birds";

// 跟 user["likes birds"] = true; 一样
user[key] = true;
// 但不能使用 . 运算符
alert( user.key ) // undefined
```



练习：

1. 创建一个空的对象 `user`。
2. 为这个对象增加一个属性，键是 `name`，值是 `John`。
3. 再增加一个属性，键是 `surname`，值是 `Smith`。
4. 把键为 `name` 的属性的值改成 `Pete`。
5. 删除这个对象中键为 `name` 的属性。

```javascript
let user = {};
user.name = "John";
user.surname = "Smith";
user.name = "Pete";
delete user.name;
```



### JavaScript 的垃圾回收

- 垃圾回收是自动完成的，我们不能强制执行或是阻止执行。
- 当对象是可达状态时，它一定是存在于内存中的。
- 被引用与可访问（从一个根）不同：一组相互连接的对象可能整体都不可达。



### 对象方法，"this"

- 存储在对象属性中的函数被称为“方法”
- 方法允许对象进行像 `object.doSomething()` 这样的“操作”
- 方法可以将对象引用为 `this`
- **`this` 的值是在程序运行时得到的**

- 一个函数在声明时，可能就使用了 `this`，但是这个 `this` 只有在函数被调用时才会有值
- 可以在对象之间复制函数
- 以“方法”的语法调用函数时：`object.method()`，调用过程中的 `this` 值是 `object`
- **lambda 函数没有 `this`，在 lambda 函数内部访问到的 `this` 都是从外部获取的**



```javascript
// 方法简写看起来更好，对吧？
let user = {
  sayHi() { // 与 "sayHi: function()" 一样
    alert("Hello");
  }
};

// lambda 函数没有自己的 this
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```



### 构造函数

1. 构造函数，或简称构造器，就是常规函数，但**命名首字母要大写**
2. 构造函数只能使用 `new` 来调用



```javascript
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

let john = new User("John");

john.sayHi(); // My name is: John

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```



### 使用 `?.` 安全的访问嵌套对象属性

使用 `?.` 来安全地读取或删除，但不能写入；

例如 `value?.prop`：

- 如果 `value` 存在，则结果与 `value.prop` 相同，
- 否则（当 `value` 为 `undefined/null` 时）则返回 `undefined`。



即使 对象 `user` 不存在，使用 `user?.address` 来读取地址也没问题：

```javascript

let user = null;

alert( user?.address ); // undefined
alert( user?.address.street ); // undefined
```



`?.` 的使用场合：

- ?. 前的变量必须已声明
- 应该只将 ?. 使用在一些东西可以不存在的地方



#### 其它变体：`?.()`，`?.[]`

 `?.()` 用于调用一个可能不存在的函数：

```javascript
let userAdmin = {
  admin() {
    alert("I am admin");
  }
};

let userGuest = {};

userAdmin.admin?.(); // I am admin

userGuest.admin?.(); // 啥都没有（没有这样的方法）
```



`?.[]` 用于从一个可能不存在的对象上安全地读取属性：

```javascript
let user1 = {
  firstName: "John"
};

let user2 = null; // 假设，我们不能授权此用户

let key = "firstName";

alert( user1?.[key] ); // John
alert( user2?.[key] ); // undefined

alert( user1?.[key]?.something?.not?.existing); // undefined
```



### 对象 — 原始值转换

对象到原始值的转换，是由许多期望以原始值作为值的内建函数和运算符自动调用的。

这里有三种类型（hint）：

- `"string"`（对于 `alert` 和其他需要字符串的操作）
- `"number"`（对于数学运算）
- `"default"`（少数运算符）



规范明确描述了哪个运算符使用哪个 hint。很少有运算符“不知道期望什么”并使用 `"default"` hint。通常对于内建对象，`"default"` hint 的处理方式与 `"number"` 相同，因此在实践中，最后两个 hint 常常合并在一起。

转换算法是：

1. 调用 `obj[Symbol.toPrimitive](hint)` 如果这个方法存在，

2. 否则，如果 hint 是 

   ```
   "string"
   ```

   - 尝试 `obj.toString()` 和 `obj.valueOf()`，无论哪个存在。

3. 否则，如果 hint 是 

   ```
   "number"
   ```

    或者 

   ```
   "default"
   ```

   - 尝试 `obj.valueOf()` 和 `obj.toString()`，无论哪个存在。

在实践中，为了便于进行日志记录或调试，对于所有能够返回一种“可读性好”的对象的表达形式的转换，只实现以 `obj.toString()` 作为全能转换的方法就够了。



### 对象原型（prototype）

内容来自：https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object_prototypes

JavaScript 常被描述为一种**基于原型的语言 (prototype-based language)**——每个对象拥有一个**原型对象**，对象以其原型为模板、从原型继承方法和属性。原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为**原型链 (prototype chain)**，它解释了为何一个对象会拥有定义在其他对象中的属性和方法。





# JSON 方法，toJSON

[JSON](http://en.wikipedia.org/wiki/JSON)（JavaScript Object Notation）是表示值和对象的通用格式。在 [RFC 4627](http://tools.ietf.org/html/rfc4627) 标准中有对其的描述。最初它是为 JavaScript 而创建的，但许多其他编程语言也有用于处理它的库。因此，当客户端使用 JavaScript 而服务器端是使用 Ruby/PHP/Java 等语言编写的时，使用 JSON 可以很容易地进行数据交换。

JavaScript 提供了如下方法：

- `JSON.stringify` 将对象转换为 JSON。
- `JSON.parse` 将 JSON 转换回对象。



得到的 `json` 字符串是一个被称为 **JSON 编码（JSON-encoded）** 或 **序列化（serialized）** 或 **字符串化（stringified）** 或 **编组化（marshalled）** 的对象，我们现在已经准备好通过有线发送它或将其放入普通数据存储。

请注意，JSON 编码的对象与对象字面量有几个重要的区别：

- 字符串使用双引号。JSON 中没有单引号或反引号。所以 `'John'` 被转换为 `"John"`
- 对象属性名称也是双引号的。这是强制性的。所以 `age:30` 被转换成 `"age":30`



## 将对象转换为 JSON：`JSON.stringify()` 函数

大部分情况，`JSON.stringify` 仅与第一个参数一起使用，但是，如果我们需要微调替换过程，比如过滤掉循环引用，我们可以使用 `JSON.stringify` 的第二个参数

```javascript
let json = JSON.stringify(value[, replacer, space])
```

- value：要编码的值。
- replacer：要编码的属性数组或映射函数 `function(key, value)`。
- space：用于格式化的空格数量



```javascript
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

let json = JSON.stringify(student);

alert(typeof json); // we've got a string!

alert(json);
/* JSON 编码的对象：
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/
```



### 排除和转换：replacer

可以为 `occupiedBy` 以外的所有内容按原样返回 `value`，为了 `occupiedBy`，下面的代码返回 `undefined`：         

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup 引用了 room
};

room.occupiedBy = meetup; // room 引用了 meetup

alert( JSON.stringify(meetup, function replacer(key, value) {
  alert(`${key}: ${value}`);
  return (key == 'occupiedBy') ? undefined : value;
}));

/* key:value pairs that come to replacer:
:             [object Object]
title:        Conference
participants: [object Object],[object Object]
0:            [object Object]
name:         John
1:            [object Object]
name:         Alice
place:        [object Object]
number:       23
*/
```



### 格式化：space

指定嵌套对象缩进几个空格，默认值是2个空格；



### 对象可以提供 `toJSON` 方法

```javascript
let room = {
  number: 23,
  // 添加一个自定义的 toJSON：
  toJSON() {
    return this.number;
  }
};

let meetup = {
  title: "Conference",
  room
};

alert( JSON.stringify(room) ); // 23

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "room": 23
  }
*/
```



## 要解码 JSON 字符串：`JSON.parse()` 函数

```javascript
let value = JSON.parse(str, [reviver]);
```

- str

  要解析的 JSON 字符串。

- reviver

  可选的函数 function(key,value)，该函数将为每个 `(key, value)` 对调用，并可以对值进行转换。



例：

```javascript
let userData = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

let user = JSON.parse(userData);

alert( user.friends[1] ); // 1
```



### 使用 reviver

reviver 函数适用于 **反序列（deserialize）**，即转换回 JavaScript 对象：

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( meetup.date.getDate() ); // 现在正常运行了！
```



# Thinking in React

内容来自：https://beta.reactjs.org/learn/thinking-in-react 



## Props vs State

React 有两种类型的“模型”数据：Props 和 State

- Props 是传递给函数的参数，例如一个 `Form` 可以通过一个 `color`支持一个 `Button` 
- State 是组件需要追踪并随时刷新的内容，例如一个 `Button`可能会跟踪 `isHovered`状态



## Hook

Hook 用于“挂钩”一个组件的 [渲染周期](https://beta.reactjs.org/learn/render-and-commit)

###  `useState()` Hook

```react
import { useState } from 'react';

// 添加两个状态变量 FilterableProductTable并指定应用程序的初始状态
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} 
        onFilterTextChange={setFilterText} 
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable 
        products={products} 
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.name.indexOf(filterText) === -1) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} placeholder="Search..." 
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} 
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```



## 组件

React 允许将标记、CSS 和 JavaScript 组合成自定义“组件” ，即应用程序的可重用 UI 元素



### `export default` 导出组件

`export default` 是 [JavaScript 的语法 ](https://developer.mozilla.org/docs/web/javascript/reference/statements/export)，允许在文件中标记要导出的主要功能，以便可以从其他文件中导入它



### 定义 JS 函数

React 组件是常规的 JavaScript 函数，但 **名称必须以大写字母开头**



### 返回 JSX 标记

跨行的返回信息必须用 `<div>` 包裹：

```react
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```



#### 浏览器如何理解

- `<section>` 是小写的，所以 React 知道我们指的是一个 **HTML 标签**
- `<Profile />` 以大写开头 `P`，所以 React 知道我们想要使用我们的 **组件** `Profile`

```react
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```



### 导出和导入组件 

一个文件最多只能有一个 **默认** 导出，但它可以有任意多个 **命名** 导出

<img src="https://beta.reactjs.org/images/docs/illustrations/i_import-export.svg" style="zoom: 67%;" />



|          | 导出语句                              | 导入语句                                |
| -------- | ------------------------------------- | --------------------------------------- |
| 默认导出 | `export default function Button() {}` | `import Button from './button.js';`     |
| 命名导出 | `export function Button() {}`         | `import { Button } from './button.js';` |



例：把 `Gallery` 、 `Profile` 移到单独文件，这里采用默认导出

- **App.js：**

```react
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```



- **Gallery.js：**

```react
import Profile from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}

```



- **Profile.js：**

```react
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```



## JSX

HTML 标记不能直接放进 React 组件，但可以将 HTML 标记转换为 JSX 标记；

### JSX 的规则

#### 1. 只返回单个根元素

```react
<div>	{/* 也可以写成 <> */}
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    className="photo"
  >
  <ul>
    ...
  </ul>
</div>  {/* 也可以写成 </> */}
```



#### 2. 需要显式闭合标签

`<img>` 必须写成 `<img />`

`<li>oranges` 必须写成 `<li>oranges</li>` 



#### 3. 使用驼峰命名

例外： [`aria-*`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) 和 [`data-*`](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) 属性像在 HTML 中一样用破折号编写

```react
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```



#### 4. 用引号或花括号传递字符串

```react
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"  {/* "avatar" 是字符串属性 */}
      src={avatar}        {/* 动态的文本信息，会调用变量 avatar */}
      alt={description}
    />
  );
}
```



#### 5. 在花括号间编写 JavaScript

任何 JavaScript 表达式都可以在大括号之间工作

```react
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>  {/* 函数调用 `formatDate()` */}
  );
}

{/* 甚至还可以写成这样： */}
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```



在 JSX 中只能以两种方式使用花括号： 

- 作为 **文本** 直接在 JSX 标签内
  - `<h1>{name}'s To Do List</h1>` 有效
  - `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` 是非法的
- 作为 **属性** 紧随其后的 `=` 标志
  - `src={avatar}` 将阅读 avatar变量
  - `src="{avatar}"` 将传递字符串 {avatar}



#### 6. 双层花括号传递对象

对象也用花括号表示，例： `{ name: "Hedy Lamarr", inventions: 5 }` 

因此在 JSX 中传递 JS 对象必须将该对象包裹在两层花括号中： `person={{ name: "Hedy Lamarr", inventions: 5 }}`



例：传一个背景为黑色，字体为粉色的内联样式

```react
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```



### HTML to JSX 转换器

[转换器链接](https://transform.tools/html-to-jsx) ——将您现有的 HTML 和 SVG 转换为 JSX



## Props

- React 组件使用 **props** 来相互通信；
-  每个父组件都可以通过提供 props 将一些信息传递给它的子组件；
- Props 可以传递任何 JavaScript 值，包括对象、数组和函数；



### 给子组件传 Props

这里子组件是 Avatar：

```javascript
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```



### 子组件接收 Props 

- 也可以设默认参数，这里设了size=100；
- 组件需要更改其 props（例如，响应用户交互或新数据）时，它的旧 props 将被丢弃，然后它要求其父组件传递一个新 props，最终 JavaScript 引擎将回收旧 props 占用的内存；

```javascript
function Avatar({ person, size=100 }) {
  // person and size are available here
    return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

// 也可以：
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```



## JSX 条件渲染 

在 React 中，控制流（如条件）由 JavaScript 处理，可以根据条件返回不同的 JSX 

例：Sally Ride 的装箱单，有一个 `PackingList`组件渲染几个 `Item`s

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```



