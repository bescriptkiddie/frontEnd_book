## js 篇

#### 数组处理函数有哪些？ 创建数组新引用有哪些？

```js
// every(): 数组的每一项都满足给定条件则返回true
var arr = [1, 2, 3, 4, 5];
var everyResult = arr.every(function (item, index, array) {
  return item > 2;
});

console.log(everyResult); // false

// some(): 数组中只需有一项满足给定条件则返回true
var arr = [1, 2, 3, 4, 5];
var everyResult = arr.some(function (item, index, array) {
  return item > 2;
});

console.log(everyResult); // true

// filter(): 返回所有满足给定条件的数据项所组成的新数组
var arr = [1, 2, 3, 4, 5];
var everyResult = arr.filter(function (item, index, array) {
  return item > 2;
});

console.log(everyResult); // [3, 4, 5]
console.log(arr); // [1,2,3,4,5]

// map()：对数组的每一项应用给定条件，返回新的数组
var arr = [1, 2, 3, 4, 5];
var everyResult = arr.map(function (item, index, array) {
  return item * 2;
});

console.log(everyResult); // [2, 4, 6, 8, 10]
console.log(arr); // [1, 2, 3, 4, 5]

// forEach(): 数组遍历，与for循环一样
var arr = [1, 2, 3, 4, 5];
arr.forEach(function (item, index, array) {
  // 执行某些操作
});

// reduce()和reduceRight(), 这两个方法只是遍历方向不同
var arr = [1, 2, 3, 4, 5];
var sum = arr.reduce(function (prev, cur, index, array) {
  return prev + cur;
}, 0);
// 注意初始值不设的话，遇到空数组会报错
console.log(sum); // 15
```

#### es6 let const 区别？

let 和 const 的相同点:

1.只在声明所在的块级作用域内有效。

2.不提升，同时存在暂时性死区，只能在声明的位置后面使用。

3.不可重复声明。

let 和 const 的不同点:

1.let 声明的变量可以改变，值和类型都可以改变；const 声明的常量不可以改变，这意味着，const 一旦声明，就必须立即初始化，不能以后再赋值;

```js
const i ; // 报错，一旦声明，就必须立即初始化
const j = 5;
j = 10; // 报错，常量不可以改变
```

2.数组和对象等复合类型的变量，变量名不指向数据，而是指向数据所在的地址。const 只保证变量名指向的地址不变，并不保证该地址的数据不变，所以将一个复合类型的变量声明为常量必须非常小心

```js
const arr = [];
// 报错，[1,2,3]与[]不是同一个地址
arr = [1, 2, 3];
const arr = [];

// 不报错，变量名arr指向的地址不变，只是数据改变
arr[0] = 1;
arr[1] = 2;
arr[2] = 3;
console.log(arr.length); // 输出：3

// 若想让定义的对象或数组的数据也不能改变，可以使用object.freeze(arr)进行冻结。冻结指的是不能向这个对象或数组添加新的属性，不能修改已有属性的值，不能删除已有属性。
const arr = [];
Object.freeze(arr);

// 不报错，但数据改变无效
arr[0] = 1;
arr[1] = 2;
arr[2] = 3;
console.log(arr.length); // 输出：0

var tmp = new Date();
function f() {
  console.log(tmp); // 想打印外层的时间作用域
  if (false) {
    var tmp = "hello world"; // 这里声明的作用域为整个函数,变量提升
  }
}
f(); // undefined
```

#### js 性能优化有哪些？

1. 减少 HTTP 请求
2. 使用 HTTP2
3. 使用服务端渲染
   客户端渲染: 获取 HTML 文件，根据需要下载 JavaScript 文件，运行文件，生成 DOM，再渲染。
   服务端渲染：服务端返回 HTML 文件，客户端只需解析 HTML。
   优点：首屏渲染快，SEO 好;
4. 静态资源使用 CDN
5. 将 CSS 放在文件头部，JavaScript 文件放在底部
   所有放在 head 标签里的 CSS 和 JS 文件都会堵塞渲染。如果这些 CSS 和 JS 需要加载和解析很久的话，那么页面就空白了。所以 JS 文件要放在底部，等 HTML 解析完了再加载 JS 文件。
   那为什么 CSS 文件还要放在头部呢？
   因为先加载 HTML 再加载 CSS，会让用户第一时间看到的页面是没有样式的、“丑陋”的，为了避免这种情况发生，就要将 CSS 文件放在头部了。
   另外，JS 文件也不是不可以放在头部，只要给 script 标签加上 defer 属性就可以了，异步下载，延迟执行
6. 使用字体图标 iconfont 代替图片图标
   字体图标就是将图标制作成一个字体，使用时就跟字体一样，可以设置属性，例如 font-size、color 等等，非常方便。并且字体图标是矢量图，不会失真。还有一个优点是生成的文件特别小
7. 善用缓存，不重复加载相同的资源
8. 压缩文件
   压缩文件可以减少文件下载时间，让用户体验性更好。
   得益于 webpack 和 node 的发展，现在压缩文件已经非常方便了。
   在 webpack 可以使用如下插件进行压缩：
   JavaScript：UglifyPlugin
   CSS ：MiniCssExtractPlugin
   HTML：HtmlWebpackPlugin
   其实，我们还可以做得更好。那就是使用 gzip 压缩。可以通过向 HTTP 请求头中的 Accept-Encoding 头添加 gzip 标识来开启这一功能。当然，服务器也得支持这一功能。
   gzip 是目前最流行和最有效的压缩方法。举个例子，我用 Vue 开发的项目构建后生成的 app.js 文件大小为 1.4MB，使用 gzip 压缩后只有 573KB，体积减少了将近 60%。
9. 图片优化
   图片延迟加载,在页面中，先不给图片设置路径，只有当图片出现在浏览器的可视区域时，才去加载真正的图片，这就是延迟加载。对于图片很多的网站来说，一次性加载全部图片，会对用户体验造成很大的影响，所以需要使用图片延迟加载。
10. 通过 webpack 按需加载 JavaScript 代码
11. 使用事件委托
12. if-else 对比 switch
    当判断条件数量越来越多时，越倾向于使用 switch 而不是 if-else。

#### Object 上有哪些常用的函数？

1.hasOwnProperty(propertyName)

hasOwnProperty 方法接收一个字符串参数，该参数表示属性名称，用来判断该属性是否在当前对象实例中，而不是在对象的原型链中。直白点儿说就是检测当前对象有没有某个属性，返回一个布尔值

```js
var arr = [];
console.log(arr.hasOwnProperty("length")); // true
console.log(arr.hasOwnProperty("hasOwnProperty")); // false
```

2.isPrototypeOf(Object)

isPrototype 方法接收一个对象，用来判断当前对象是否在传入的参数对象的原型链上，返回一个布尔值

这个检测的就是 Object 的原型对象是否在 obj 的原型链上，MyObject 是继承自 Object 对象的，而在 JS 中，继承是通过 prototype 来实现的，所以 Object 的 prototype 必定在 MyObject 对象实例的原型链上。所以结果是肯定的

```js
function MyObject() {}
var obj = new MyObject();
console.log(Object.prototype.isPrototypeOf(obj)); // true
```

3.propertyIsEnumerable(prototypeName)
prototypeIsEnumerable 用来判断给定的属性是否可以被 for..in 语句给枚举出来(个人感觉可以理解为是否可以被 for..in 语句遍历到)出来，返回一个布尔值

```js
var obj = {
  name: "objName",
};
for (var i in obj) {
  console.log(i); // name
}
console.log(obj.propertyIsEnumerable("constructor")); // false
```

执行这段代码输出字符串“name”，这就说明通过 for…in 语句可以得到 obj 的 name 这个属性，但是，obj 的属性还有很多，比如 constructor，比如 hasOwnPrototype 等等，它们并没有被输出，说明这些属性不能被 for…in 给枚举出来，可以通过 propertyIsEnumerable 方法来得到。

4.toLocaleString()

toLocalString 方法返回对象的字符串表示，和代码的执行环境有关。

```js
var obj = {};
console.log(obj.toLocaleString()); // [object Object]

var date = new Date();
console.log(date.toLocaleString()); // 2020/4/26 上午11:04:50 输出的就是本地的时间
```

5.toString()
toString 用来返回对象的字符串表示

```js
var obj = {};
console.log(obj.toString()); // [object Object]

var date = new Date();
console.log(date.toString()); // Sun Apr 26 2020 11:04:35 GMT+0800 (中国标准时间)
```

6.valueOf()
valueOf 方法返回对象的原始值，根据对象的不同，返回的可能是字符串、数值或 boolean 值等

```js
var obj = {
  name: "obj",
};
console.log(obj.valueOf()); // Object {name: "obj"}

var arr = [1];
console.log(arr.valueOf()); // [1]

var date = new Date();
console.log(date.valueOf()); // 1456638436303
```

#### js 在 Object 常用属性

1.prototype(原型对象)
构造函数有一个 prototype 属性，指向实例对象的原型对象。通过同一个构造函数实例化的多个对象具有相同的原型对象。(经常使用原型对象来实现继承)

```js
function Foo() {}
Foo.prototype.a = 1;
var f1 = new Foo();
var f2 = new Foo();

console.log(Foo.prototype.a); // 1
console.log(f1.a); // 1
console.log(f2.a); // 1
```

2.constructor
原型对象有一个 constructor 属性，指向该原型对象对应的构造函数

```
function Foo() {};
console.log(Foo.prototype.constructor === Foo); // true
```

3.\_\_proto\_\_
这个东西并不是标准的原型，它是一些浏览器提供的一个查看 prototype 的接口，多用于调试，prototype 才是标准原型，可用在代码中。

```
function Foo(){};
var f1 = new Foo;
console.log(f1.__proto__ === Foo.prototype); // true
```

#### 事件委托机制

事件委托就是利用冒泡的原理，把事件加到父元素或祖先元素上，触发执行效果。
1、提高 JavaScript 性能。事件委托可以显著的提高事件的处理速度，减少内存的占用
2、 动态绑定事件

```js
<ul id="list">
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
  ......
  <li>item n</li>
</ul>;

// 给父层元素绑定事件
document.getElementById("list").addEventListener("click", function (e) {
  // 兼容性处理
  var event = e || window.event;
  var target = event.target || event.srcElement;

  // 判断是否匹配目标元素
  if (target.nodeName.toLocaleLowerCase === "li") {
    console.log(target.innerHTML);
  }
});
// target 元素则是在 #list 元素之下具体被点击的元素，然后通过判断 target 的一些属性（比如：nodeName，id 等等）
```

局限性

1、focus、blur 之类的事件本身没有事件冒泡机制，所以无法委托

2、mousemove、mouseout 虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，也不适合于事件委托

#### 事件冒泡，阻止冒泡事件，阻止默认事件

首先说明什么是事件冒泡？当事件发生后，这个事件就要开始传播（从里到外，或者从外向里）。为什么要传播呢？因为事件源本身（可能）并没有处理事件的能力，或处理事件的函数并未绑定在该事件源上

阻止冒泡事件

```js
function bubbles(e) {
  var ev = e || window.event;
  if (ev && ev.stopPropagation) {
    //非IE浏览器
    ev.stopPropagation();
  } else {
    //IE浏览器(IE11以下)
    ev.cancelBubble = true;
  }
  console.log("最底层盒子被点击了");
}
```

阻止默认事件

```js
// 谷歌及IE8以上
e.preventDefault();

// IE8及以下
window.event.returnValue = false;

return false;
```

#### ES6 异步请求方法 promise/async await/generator 等

类似于将一个异步方法封装在一个具有回调函数的函数里，Promise 实际上充当了这种封装作用。然后通过 resolve 和 reject 函数向外输出成功时的数据和失败时的错误信息

```js
const p = new Promise((resolve, reject) => {
  setTimeout(function () {
    const name = "joyitsai";
    resolve(name);
    /*
  如果失败reject输出错误信息
    reject(`error_info`)
  */
  }, 1000);
});
p.then((data) => {
  console.log(data);
});
```

Promise，把它想象成一个容器，里面放着一些正在处理的问题，但不管过多长时间，它最终都会把它处理完成的结果(不管是成功还是失败的)输出给你，而且，它允许你通过.then((data)=>{})方法来接收这个结果数据。Promise 可以说是为处理异步而生.

1.关于 async

它是将一个方法或函数变成异步的，它会将一个普通函数封装成一个 Promise，为什么要封装成 Promise 呢？其实它只是 Promise 的搬运工，利用了 Promise 处理异步问题的能力，能让你更好地解决需要异步处理的代码

```js
/**async/await 与Promise */
async function testAsync() {
  return "Here is Async";
}
const result = testAsync();
console.log(result); // 输出 Promise { 'Here is Async' }
```

2.关于 await

async 用来申明里面包裹的内容可以进行同步的方式执行，await 则是进行执行顺序控制，每次执行一个 await，程序都会暂停等待 await 返回值，然后再执行之后的 await。

await 只能用在 async 函数之中，用在普通函数中会报错

await 命令后面的 Promise 对象，运行结果可能是 rejected，所以最好把 await 命令放在 try...catch 代码块中

await 具有阻塞功能，可以理解为：等待 await 语句执行完成后，后面的程序才会继续。这就意味着，虽然一段包含 await 语句的异步程序，最终却是按照同步的顺序来执行

```js
async function test() {
  console.log(2);
  return "Joyitsai";
}

async function run() {
  console.log(1);
  const data = await test();
  console.log(data);
  console.log(3);
}
run();

// 打印结果
1;
2;
Joyitsai;
3;
```

3.ES6 中的 Generator 函数

Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同。从语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态

形式上，Generator 函数是一个普通函数，但是有两个特征：1.function 关键字与函数名之间有一个星号 2.函数体内部使用 yield 语句，定义不同的内部状态

```js
var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  var length = a.length;
  for (var i = 0; i < length; i++) {
    var item = a[i];
    if (typeof item !== "number") {
      yield* flat(item);
    } else {
      yield item;
    }
  }
};

for (var f of flat(arr)) {
  console.log(f);
}
// 1, 2, 3, 4, 5, 6
```

#### var let 和 const 区别

1. 块级作用域
   ES5 只有全局作用域和函数作用域，没有块级作用域。
   这带来很多不合理的场景: 1).内层变量可能覆盖外层变量, 2).用来计数的循环变量泄露为全局变量

```js
var tmp = new Date();
function f() {
  console.log(tmp); // 想打印外层的时间作用域
  if (false) {
    var tmp = "hello world"; // 这里声明的作用域为整个函数
  }
}
f(); // undefined
```

2. 不存在变量提升
   变量提升的现象：在同一作用域下，变量可以在声明之前使用，值为 undefined
   ES5 时使用 var 声明变量，经常会出现变量提升的现象。

```
// var 的情况
console.log(foo); // 输出undefined
var foo = 8;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 8;
```

3. 暂时性死区
   只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量

```
var tmp = 888;
if (true) {
  tmp = '999'; // 报错 因为本作用域内有tmp声明变量
  let tmp; // 绑定if这个块级的作用域不能出现tmp变量
}
```

暂时性死区和不能变量提升的意义在于: 为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。

4. 不允许重复声明变量
   let、const 不允许在相同作用域内，重复声明同一个变量
5. let、const 声明的  全局变量不会挂在顶层对象下面
   var 声明的全局变量会挂在顶层对象下面，而 let、const 不会挂在顶层对象下面。如下面这个栗子

```
var a = 8;
// 如果在 Node环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 8

let b = 8;
window.b // undefined
```

#### 什么是作用域链 作用域 闭包?

作用域
先来谈谈变量的作用域
变量的作用域无非就是两种：全局变量和局部变量。
全局作用域：
最外层函数定义的变量拥有全局作用域，即对任何内部函数来说，都是可以访问的：

```js
<script>
  var outerVar = "hello";
  function fn(){
    console.log(outerVar);
  }
  fn(); // hello
</script>

var a = 1;
function fn(a){
    console.log(a)
    var a = 2;
    function a(){}
}
fn(a); // ƒ a(){} 函数在生命时，既生明也定义

var foo='hello';
(function(foo){
   console.log(foo);
   var foo=foo||'world';
   console.log(foo);
})(foo);
console.log(foo); // hello hello hello  变量提升有形参赋值，以形参赋值为准

```

局部作用域：
和全局作用域相反，局部作用域一般只在固定的代码片段内可访问到，而对于函数外部是无法访问的，最常见的例如函数内部

```js
<script>
  function fn(){
    var innerVar = "inner";
  }
  fn();
  console.log(innerVar); // ReferenceError: innerVar is not defined
</script>
```

#### 实现一个函数组合

在函数式编程当中有一个很重要的概念就是函数组合， 实际上就是把处理数据的函数像管道一样连接起来， 然后让数据穿过管道得到最终的结果。 例如：

```js
    const add1 = (x) => x + 1;
    const mul3 = (x) => x * 3;
    const div2 = (x) => x / 2;
    div2(mul3(add1(add1(0)))); //=>3
​
    // 而这样的写法可读性明显太差了，我们可以构建一个compose函数，它接受任意多个函数作为参数（这些函数都只接受一个参数），然后compose返回的也是一个函数，达到以下的效果：
    const operate = compose(div2, mul3, add1, add1)
    operate(0) //=>相当于div2(mul3(add1(add1(0))))
    operate(2) //=>相当于div2(mul3(add1(add1(2))))
​
   //  简而言之：compose可以把类似于f(g(h(x)))这种写法简化成compose(f, g, h)(x)，请你完成 compose函数的编写

   function compose() {
     let args = argumnets;
     let start = args.length-1;
     return function() {
       let i = start;
       let result = args[i].apply(this, arguments);
       while(i-- && i >= 0) {
         result = args[i].call(this, result);
       }
       return result;
     }
   }
```

#### 箭头函数的概念和普通函数的区别?

箭头函数的 this 指向规则：

1. 箭头函数没有 prototype(原型)，所以箭头函数本身没有 this

```
let a = () =>{};
console.log(a.prototype); // undefined
```

2. 箭头函数的 this 指向在定义的时候继承自外层第一个普通函数的 this。
3. 不能直接修改箭头函数的 this 指向
4. 箭头函数外层没有普通函数，严格模式和非严格模式下它的 this 都会指向 window(全局对象)

```js
const obj = {
  array: [1, 2, 3],
  sum: () => {
    // 外层没有普通函数this会指向全局对象
    return this.array.push('全局对象下没有array，这里会报错'); // 找不到push方法
  }
};
obj.sum();

// 修改后 这两种写法是等价的
sum() {
  return this.array.push('this指向obj');
}
sum: function() {
  return this.array.push('this指向obj');
}
```

5. 使用 new 调用箭头函数会报错
   无论箭头函数的 this 指向哪里，使用 new 调用箭头函数都会报错，因为箭头函数没有 constructor

```js
let a = () => {};
let b = new a(); // a is not a constructor
```

#### js 的 toFixed 方法

Number.toFixed(num) 方法可把 Number 四舍五入为指定小数位数的数字
num 必需。规定小数的位数，是 0 ~ 20 之间的值，包括 0 和 20
返回值：字符串 string

#### js 的 map 方法

map 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值

方法按照原始数组元素顺序依次处理元素

不会对空数组进行检测,不会改变原始数组,兼容 ie9+

#### ES6 都有哪些新的 api，每一个都详细谈谈

includes 第一个参数表示需要查找的字符串，第二个参数表示查找的起始下标位置

```js
let a = "abc";
let result1 = a.includes("a"); // true
let result2 = a.includes("d"); // false
let result3 = a.includes("a", 1); // false
```

startsWith 第一个参数表示需要查找的字符串，第二个参数表示查找的起始下标位置

```js
let a = "abc";
let result1 = a.startsWith("a"); // true
let result2 = a.startsWith("b"); // false
let result3 = a.startsWith("b", 1); // true
```

数值的方法
Number.isInteger()

```js
Number.isInteger(2); // true
Number.isInteger(2.1); // false
```

Array.form()方法
用于将类数组的对象转为真实的数组，比如 dom

```js
let domList = document.getElementsByTagName("div");
let result = Array.from(domList);
console.log(resule, result instanceof Array); // [div, div, div] true
```

Array.of()
用于将一组值转为数组

```js
let a = "123";
let b = "456";
let result = Array.of(a, b);
console.log(result); // ['123', '456']
```

#### 判断数据类型的几种方法，优缺点，实现方式

1.typeof 直接返回数据类型字符串，无法判断数组，对象，null,其中 null、{}、[] 都返回 object
2.instanceof 判断某个实例是不是属于原型

#### 算法，最大连续子序列(dp)

当子序列的某个元素之前的元素和为负数时，他对后边的最大和一定是一个负向增益，没有该元素本身大,利用这个特点，我们把目光投向整个数组，如果我们从前向后遍历，当遇到前方和为负数时，就可以抛点前边的元素，从当前元素继续向后去计算，也可以总结成一个动态规划的公式:`dp = Math.max(dp + current, current)`

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
let maxSubArray = function (nums) {
  // 默认当前的最大和为第一个元素
  let sum = nums[0];
  // dp代表以当前元素结尾的最大和，默认也是第一个元素
  let dp = nums[0];
  // 从数组的第二个元素开始循环
  for (let i = 1; i < nums.length; i++) {
    // 上文中提到的dp公式
    dp = Math.max(dp + nums[i], nums[i]);
    // 同时进行当前最大值的记录
    sum = Math.max(sum, dp);
  }
  return sum;
};
```

#### instanceOf 原理，手写一个 instanceOf

instanceof 可以检测某个对象是不是另一个对象的实例
如果一个对象是数组，应该怎么判断？用 arr instanceof Array

#### 二维码扫描登录实现原理

移动端基于 token 的认证机制。
二维码扫码登录的原理。

#### 大体积文件上传【分片、断点续传】

使用 webuploader 组件实现大文件分片上传，断点续传,webuploader：是一个以 HTML5 为主， Flash 为辅的文件上传组件，采用大文件分片/并发上传的方式，极大地提高了文件上传的效率，同时兼容多种浏览器版本；

#### treeshaking 原理

Tree-shaking 的本质是消除无用的 js 代码。无用代码消除在广泛存在于传统的编程语言编译器中，编译器可以判断出某些代码根本不影响输出
ES6 模块依赖关系是确定的，和运行时的状态无关，可以进行可靠的静态分析，这就是 tree-shaking 的基础。

#### node 如何捕获异常

#### 尾递归函数优化

#### 给定一个整数数组和一个目标值，找出数组中和为目标值的两个数

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

```js
const twoNum = (nums, target) => {
  const maps = {};
  for (let i = 0, len = nums.length; i < len; i++) {
    let diff = target - nums[i];
    if (maps[diff] > -1) {
      return [maps[diff], i];
    }
    maps[nums[i]] = i;
  }
  return [];
};
```

#### redux 的设计

#### 闭包 什么时候用,有什么特点

1、函数嵌套函数

2、内部函数可以访问外部函数的变量

3、参数和变量不会被回收。

```js
function test() {
  var a = 0;
  return function () {
    a++;
    alert(a);
  };
}
var atest = new test(); //引用返回的函数
atest(); // 1
```

#### 箭头函数和普通函数有什么区别

.箭头函数相当于匿名函数，是不能作为构造函数的，不能使用 new 2.箭头函数不绑定 arguments,取而代之用 rest 参数…解决 3.箭头函数会捕获其所在上下文的 this 值，作为自己的 this 值。即箭头函数的作用域会继承自外围的作用域。 4.箭头函数当方法使用的时候没有定义 this 的绑定
.箭头函数没有函数原型

```js
obj = {
  a: 10,
  b: () => {
    console.log(this.a); //undefined
    console.log(this); //window
  },
  c: function () {
    return () => {
      console.log(this.a); //10
    };
  },
};
obj.b();
obj.c(); // 10
```

#### JS 原型链与继承

> 每个构造函数(constructor)都有一个原型对象(prototype),原型对象都包含一个指向构造函数的指针,而实例(instance)都包含一个指向原型对象的内部指针.

确定原型和实例的关系,使用 instanceof 操作符

我们需要牢记两点：

**proto**和 constructor 属性是对象所独有的；

prototype 属性是函数所独有的，因为函数也是一种对象，所以函数也拥有**proto**和 constructor 属性。

**proto**属性的作用就是当访问一个对象的属性时，如果该对象内部不存在这个属性，那么就会去它的**proto**属性所指向的那个对象（父对象）里找，一直找，直到**proto**属性的终点 null，再往上找就相当于在 null 上取值，会报错。通过**proto**属性将对象连接起来的这条链路即我们所谓的原型链。

prototype 属性的作用就是让该函数所实例化的对象们都可以找到公用的属性和方法，即 f1.**proto** === Foo.prototype。

constructor 属性的含义就是指向该对象的构造函数，所有函数（此时看成对象了）最终的构造函数都指向 Function。

#### CommonJS、AMD、CMD 和 ES6 模块化区别

NodeJS 是 CommonJS 规范的实现，webpack 也是以 CommonJS 的形式来书写。node.js 将 javascript 语言用于服务器端编程。

#### 代码输出

```js
function Foo() {
  Foo.a = function () {
    console.log(1);
  };
  this.a = function () {
    console.log(2);
  };
}
Foo.prototype.a = function () {
  console.log(3);
};
Foo.a = function () {
  console.log(4);
};

Foo.a(); // 4
let obj = new Foo();
obj.a(); // 2
Foo.a(); // 1
```

#### DOCTYPE 有哪些写法，有什么区别，什么是怪异模式，有哪些废弃掉的元素

#### async，await 的理解

#### 如何获取数组中最大的数

1.排序法

```js
var arr = [5, 7, 1, 3, 9];

// 从小到大排序
arr.sort(function (a, b) {
  return a - b;
});

var min = arr[0]; // 1

var max = arr[arr.length - 1]; // 9
```

2.使用 apply 实现

apply 传入的是一个数组

```js
Math.max.apply(null, [2, 9, 78]); // 78
```

3.es6 扩展运算符

```js
var arr = [5, 7, 1, 3, 9];
console.log(Math.max(...arr)); // 9
```

#### 数组和链表的使用场景

数组内元素在内存中是连续存储的，也有特例，数组内有对象、字符创、数字等，由于数组元素在内存中连续 ，访问元素很快，而删除增加需要移动其他元素。
链表在内存中并不是连续的，但是数据的增加、删除比较灵活，而读取某一个元素不好查找，要从根节点进行查找。
因此数组在进行频繁读取、访问时使用、而频繁删除、添加元素链表更适合

#### 了解哪些排序算法，说说冒泡排序和快排的区别

冒泡排序：是从最底层元素开始比较,（与其上的元素比较）
小于就往上再比,大于就交换,再用较小的往上比较,直到最高层

```js
function sort1(arr) {
  const length = arr.length;
  for (let i = 0; i < length; i++) {
    for (let j = 0; j < length - 1 - i; j++) {
      let temp;
      if (arr[j] > arr[j + 1]) {
        temp = arr[j + 1];
        arr[j + 1] = arr[j];
        arr[j] = temp;
        // 不引入地三个变量可使用
        // [arr[j + 1], arr[j]] = [arr[j], arr[j + 1]];
      }
    }
  }
  return arr;
}
```

快速排序：是先找到一个轴值,比较时把所有比轴值小的放到轴值的左边,
比轴值大的放到右边,再在两边各自选取轴值再按前面排序,直到完成

#### XSS 和 CSRF 攻击

> XSS，即 Cross Site Script，中译是跨站脚本攻击。XSS 本质是 Html 注入，攻击者在网站上注入恶意的 js 代码，对客户端页面进行篡改，进而窃取隐私数据比如 cookie、session，或者重定向到不好的网站等。

- XSS 场景再现:

使用 url 参数攻击：https://www.baidu.com?jarttoTest=<script>alert(document.cookie)</script>，这种是反射型的 XSS，攻击是一次性的。简单来说就是：引导用户点击恶意链接，链接上的 js 代码被发送至服务器，而服务器将不加处理的脚本又返回至客户端，此时用户客户端就会执行 js 代码。

有人在留言内容中插入恶意 js，服务器将该内容放入数据库中，此时，每当有人访问这个留言，就会执行这个恶意 js。这是存储型 XSS。

- XSS 注入方法

在 HTML 中内嵌的文本中，恶意内容以 script 标签形成注入。在内联的 JavaScript 中，拼接的数据突破了原本的限制（字符串，变量，方法名等）

在标签属性中，恶意内容包含引号，从而突破属性值的限制，注入其他属性或者标签。
在标签的 href、src 等属性中，包含 javascript: 等可执行代码。

在 onload、onerror、onclick 等事件中，注入不受控制代码。
在 style 属性和标签中，包含类似 background-image:url("javascript:..."); 的代码（新版本浏览器已经可以防范）。

在 style 属性和标签中，包含类似 expression(...) 的 CSS 表达式代码（新版本浏览器已经可以防范）。

防范措施：
不要相信用户输入： 对用户输入内容进行过滤。对特殊字符进行实体转义。
不要完全信任服务端：对服务端输出进行转义。

使用 HttpOnly Cookie：将重要的 cookie 标记为 httponly，这样就无法使用 js 代码获取 cookie。

需要转义的字符有

- CSRF
  > 即 Cross Site Request Forgery，中译是跨站请求伪造。未经用户许可，偷偷的使用用户名义，发送恶意请求的攻击。通常情况下借助用户 cookie 来骗取服务器信任。

CSRF 特点:
CSRF（通常）发生在第三方域名。
CSRF 攻击者（通常）不能获取到 Cookie 等信息，只是使用。

防范措施：
同源监测
CSRF Token: 需要服务端生成一个 Token，然后放在页面中，页面提交请求的时候，带上这个 Token。服务端  把 Token 从 Session 中拿出，与请求中的 Token 进行比对验证。

#### 伪类和伪元素的区别

伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过:hover 来描述这个元素的状态。虽然它和普通的 css 类相似，可以为已有的元素添加样式，但是它只有处于 dom 树无法描述的状态下才能为元素添加样式，所以将其称为伪类。

伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过:before 来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

区别

伪类的操作对象是文档树中已有的元素，而伪元素则创建了一个文档树外的元素。因此，伪类与伪元素的区别在于：有没有创建一个文档树之外的元素。

CSS3 规范中的要求使用双冒号(::)表示伪元素，以此来区分伪元素和伪类，比如::before 和::after 等伪元素使用双冒号(::)，:hover 和:active 等伪类使用单冒号(:)。除了一些低于 IE8 版本的浏览器外，大部分浏览器都支持伪元素的双冒号(::)表示方法。

#### document.ready 和 window.onload 的区别

document.ready 和 window.onload 的区别是：上面定义的 document.ready 方法在 DOM 树加载完成后就会执行，而 window.onload 是在页面资源（比如图片和媒体资源，它们的加载速度远慢于 DOM 的加载速度）加载完成之后才执行。也就是说\$(document).ready 要比 window.onload 先执行。

onload 怎么用：

浏览器加载完 DOM 后，会通过 javascript 为 DOM 元素添加事件，在 javascript 中，通常使用 window.onload()方法

#### 实现 Promise.retry，成功后 resolve 结果，失败后重试，尝试超过一定次数才真正的 reject

```js
Promise.retry = function (fn, num = 3) {
  return new Promise(async function (resolve, reject) {
    while (num) {
      try {
        let result = await fn();
        resolve(result);
        num = 0;
      } catch (error) {
        if (!num) {
          reject(error);
        }
      }
      num--;
    }
  });
};

function getProm() {
  const n = Math.random();
  return new Promise((resolve, reject) => {
    setTimeout(() => (n > 0.8 ? resolve(n) : reject(n)), 1000);
  });
}

Promise.retry(getProm);
```

#### 如何模拟实现 Array.prototype.splice
