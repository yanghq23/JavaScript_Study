# JavaScript_Study
# ECMAScript
@(JavaScript)[弱类型, 动态, 解释型, 脚本语言]

[TOC]
## JavaScript的一些概念
#### ECMAScript 和 JavaScript 的关系 
1996 年 11 月，JavaScript 的创造者 Netscape 公司，将 JavaScript 提交给标准化组织 ECMA，希望这种语言能够成为国际标准。次年，ECMA 发布 262 号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为 ECMAScript，这个版本就是 1.0 版。
 JavaScript 和 ECMAScript 通常被用来表达相同的含义，但 JavaScript的含义却比 ECMA-262中规定的要多得多。一个完整的 JavaScript 实现应该由下列三个部分组成。
- 核心（ECMAScript）
- 文档对象模型（DOM）
- 浏览器对象模型（BOM） 
#### 作用域和闭包
##### 作用域
在ES5中，只有全局作用域和函数作用域，并没有块作用域。
```
var variable = "itclan";        //全局变量
console.log("全局variable","=",variable); // 全局variable = itclan
// 函数表达式
var myFun = function(){
	var variable = "itclanCode"; //局部变量
	console.log("局部variable","=",variable); // 局部variable = itclanCode
	var otherFun = function(){
	     var variable = 24;       //局部变量
	     console.log("局部variable","=",variable);  // 局部variable = 24
	  }
       otherFun();
 }
 myFun();
```
>函数可以嵌套函数,并可以无限的嵌套下去,也就是可以创建无数的函数作用域,而javascript坏境只是用一个全局作用域。

ES5实现块作用域:
```
(function(){
	for(var i =0;i<10;i++){
		//some code
	}
})();
console.log(i);// i is not defined
```
这种写法叫做**立即调用函数表达式**（IIFE），创建了一个局部作用域，该作用域声明的变量只有在该块内有效，外部访问不了。好处就是可以做到不污染全局变量。
##### 闭包
闭包指可以访问另一个函数作用域变量的函数，一般是定义在外层函数中的内层函数。
局部变量无法共享和长久的保存，而全局变量可能造成变量污染，所以希望有一种机制既可以长久的保存变量又不会造成全局污染，这个时候就需要闭包。
```
var getNum;
function getCounter() { 
    var n = 1; 
    var inner = function () { return n++; }
    return inner;
}

getNum = getCounter();
console.log(getNum()); //1 
console.log(getNum()); //2
```

#### 变量提升
在ES6之前，JavaScript没有块级作用域(一对花括号`{}`即为一个块级作用域)，只有全局作用域和函数作用域。变量提升即将变量声明提升到它所在作用域的最开始的部分。
```
console.log(global); // undefined
var global = 'global';
console.log(global); // global
 
function fn () {
　　console.log(a); // undefined
　　var a = 'aaa';
　　console.log(a); // aaa
}
fn();
```
以上的打印结果，是由于js的变量提升。实际上上面的代码是按照以下来执行的：
```
var global; // 变量提升，全局作用域范围内，此时只是声明，并没有赋值
console.log(global); // undefined
global = 'global'; // 此时才赋值
console.log(global); // 打印出global
 
function fn () {
　　var a; // 变量提升，函数作用域范围内
　　console.log(a);
　　a = 'aaa';
　　console.log(a);
}
fn();
```
函数的两种声明方式：
- function fn(){} //函数声明式
- var fn = function(){}; //函数表达式
```
/* 函数声明式 */
(function(){
	fn();
	function fn() {
		console.log('来自函数声明式fn');
	}
})();// 来自函数声明式fn

/* 函数表达式 */
(function(){
	fn();
	var fn = function() {
		console.log('来自函数表达式fn');
	}
})(); // fn is not a function
```

#### 严格模式
ECMAscript 5添加了第二种运行模式：**严格模式**（strict mode）。这种模式使得JavaScript在更严格的条件下运行。
>使用`"use strict";`语句，进入**严格模式**。老版本的浏览器会把它当作一行普通字符串，加以忽略。IE10+及其他主流浏览器都支持严格模式。
##### 使用严格模式
1.整个脚本文件
将`"use strict";`放在脚本文件的第一行，则整个脚本都将以**严格模式**运行。如果这行语句不在第一行，则无效，整个脚本以**正常模式**运行。
```
	<script>
　　　　"use strict";
　　　　console.log("这是严格模式。");
　　</script>

　　<script>
　　　　console.log("这是正常模式。");
　　</script>
```
2.单个函数
将`"use strict";`放在函数体的第一行，则整个函数以**严格模式**运行。
```
	function strict(){
　　　　"use strict";
　　　　return "这是严格模式。";
　　}

　　function notStrict() {
　　　　return "这是正常模式。";
　　}
```

##### 语法和行为改变
1.全局变量显式声明
在正常模式中，如果一个变量没有声明就赋值，默认是全局变量。严格模式禁止这种用法，全局变量必须显式声明。
```
"use strict";

v = 1; // 报错，v未声明
for(i = 0; i < 2; i++) { // 报错，i未声明
}
```
2.禁止this关键字指向全局对象
```
　 function f(){
　　　　return !this;
　　} 
　　// 返回false，因为"this"指向全局对象，"!this"就是false

　　function f(){ 
　　　　"use strict";
　　　　return !this;
　　} 
// 返回true，因为严格模式下，this的值为undefined，所以"!this"为true。
```
3.禁止删除变量
严格模式下无法删除变量。只有configurable设置为true的对象属性，才能被删除。
```
"use strict";

var x;
delete x; // 语法错误

var o = Object.create(null, {'x': {
　　　value: 1,
　　　configurable: true
}});
delete o.x; // 删除成功
```

4.函数必须声明在顶层
严格模式只允许在全局作用域或函数作用域的顶层声明函数。不允许在非函数的代码块内声明函数。
```
	"use strict";
　　if (true) {
　　　　function f() { } // 语法错误
　　}
　　for (var i = 0; i < 5; i++) {
　　　　function f2() { } // 语法错误
　　}
```
##### 保留字
严格模式新增了一些保留字：`implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`。使用这些词作为变量名将会报错。
```
function package(protected) { // 语法错误
　　　　"use strict";
　　　　var implements; // 语法错误
　　}
```
ECMAscript5本身还规定了另一些保留字（`class`, `enum`, `export`, `extends`, `import`, `super`），以及各大浏览器自行增加的`const`保留字，也是不能作为变量名的。
#### 原型
**构造函数**
```
var Fn = function () {		// Fn就是构造函数！！！
  this.a= 1;
}
var f= new Fn();
```
`prototype`，可以向对象、函数，以自定义的方式，添加属性和方法。
```
//自定义添加属性、方法
Fn.prototype.b= 2;
Fn.prototype.c= function(n){
	console.log(n)
};
console.log( f.c("html") );					// html	
```
通过实例对象（ f ）的 `constructor `属性，可以找到此实例对象的构造函数。
```
console.log(Fn.prototype); //
console.log(Fn.prototype.constructor); // 函数 Fn
console.log(f.constructor);	// 函数 Fn
console.log(f.constructor == Fn.prototype.constructor); // true
```

```
//自定义添加属性、方法
Fn.prototype = {
	b: 18,
	c: function(n){
		console.log(n)
	}
}
console.log(f.constructor == Fn.prototype.constructor); // false
```

## ECMAScript 6


#### let和const声明
let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined。let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。
```
{
  let a = 10;
  var b = 1;
}
a // a is not defined.
b // 1
```
`for`循环的计数器，就很合适使用`let`命令。
```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
ES5
```
var a = [];
for (var i = 0; i < 10; i++) {
  (function(args){
	  a[i] = function () {
	    console.log(args);
	  }
  })(i);
}
a[6](); // 6
```
```
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```
`const`声明一个只读的常量。一旦声明，常量的值就不能改变。
```
const PI = 3.1415;
PI // 3.1415
PI = 3;
// TypeError: Assignment to constant variable.
```
上面代码表明改变常量的值会报错。

`const`声明的变量不得改变值，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。
```
const foo;
// SyntaxError: Missing initializer in const declaration
```
对于const来说，只声明不赋值，就会报错。
const的作用域与let命令相同：只在声明所在的块级作用域内有效。
```
if (true) {
  const MAX = 5;
}

MAX // Uncaught ReferenceError: MAX is not defined
//const命令声明的常量也是不提升，只能在声明的位置后面使用。

if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```
const声明的常量，也与let一样不可重复声明。
```
var message = "Hello!";
let age = 25;

// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```

#### 块级作用域
`let`实际上为 JavaScript 新增了块级作用域。
```
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
两个代码块，都声明了变量n，运行后输出 5。这表示外层代码块不受内层代码块的影响。如果两次都使用var定义变量n，最后输出的值才是 10。
外层作用域无法读取内层作用域的变量。
```
{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}
```
#### 函数、字符串的扩展

##### 函数
ES6 之前，不能直接为函数的参数指定默认值，只能采用变通的方法。
```
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
```
ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。
```
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
```
ES6 允许使用“箭头”（`=>`）定义函数。
```
var f = function (v) {
  return v;
};
// 等同于
var f = v => v;
```
如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
```
var sum = (num1, num2) => { return num1 + num2; }
```

##### 字符串
传统的 JavaScript 语言，输出模板通常是这样写的。
```
  var str = 'There are <b>' + basket.count + '</b> ' +
	  'items in your basket, ' +
	  '<em>' + basket.onSale +
	  '</em> are on sale!'
```
ES6 引入了模板字符串解决这个问题，模板字符串中嵌入变量，需要将变量名写在`${}`之中。
```
  var str = `There are <b>${basket.count}</b> items
		  in your basket, <em>${basket.onSale}</em>
		   are on sale!`;
```
#### 解构赋值
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
以前，为变量赋值，只能直接指定值。
``` javascript
let a = 1;
let b = 2;
let c = 3;
```
ES6 允许写成下面这样。
``` javascript
let [a, b, c] = [1, 2, 3];
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3

let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```

#### 类(class)
JavaScript 语言中，生成实例对象的传统方法是通过构造函数。
```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```
ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。
```
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```
Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。
```
class Point {
}

class ColorPoint extends Point {
}
```
父类的静态方法，可以被子类继承。
```
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```
constructor方法和toString方法之中，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。

子类必须在constructor方法中调用super方法，否则新建实例时会报错。因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

#### 浏览器ES6支持
>[ES6标准浏览器支持速查表](https://kangax.github.io/compat-table/es6/)
