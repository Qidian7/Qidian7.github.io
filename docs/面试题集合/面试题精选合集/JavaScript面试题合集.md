## 📒 JavaScript面试题合集

### JS中的8种数据类型及区别

#### 1.基本数据类型

```markdown
number，string，null，undefined，boolean，symbol，bigInt
```

```markdown
数据存放在栈中
```

#### 2.引用数据类型

```markdown
object,array,function,date,reg（正则）
```

```markdown
数据存放于堆中，在栈中存放数据的标识符及堆中存放数据的地址
```

------

### JS中的数据类型检测方案

#### 1.typeof

```js
console.log(typeof 1);               // number
console.log(typeof true);            // boolean
console.log(typeof 'mc');            // string
console.log(typeof Symbol)           // function
console.log(typeof function(){});    // function
console.log(typeof console.log());   // function
console.log(typeof []);              // object 
console.log(typeof {});              // object
console.log(typeof null);            // object
console.log(typeof undefined);       // undefined
```

优点：能够快速区分基本数据类型

**缺点**：**不能将Object、Array和Null区分，都返回object**

#### 2. instanceof

```js
console.log(1 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false  
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

优点：能够区分Array、Object和Function，适合用于判断自定义的类实例对象。**还可以在继承关系中用来判断一个实例是否属于它的父类型**

**缺点：Number，Boolean，String基本数据类型不能判断**

### 3. constructor

```js
let a = 1
let b = 'abc'
let c = true
console.log(a.constructor === Number) // true
console.log(b.constructor === String) // true
console.log(c.constructor === Boolean) // true
```

优点：相比于instanceof可以检测基本数据类型

#### 4. Object.prototype.toString.call()

```js
var toString = Object.prototype.toString;
console.log(toString.call(1));      //[object Number]
console.log(toString.call(true));   //[object Boolean]
console.log(toString.call('mc'));   //[object String]
console.log(toString.call([]));     //[object Array]
console.log(toString.call({}));     //[object Object]
console.log(toString.call(function(){})); //[object Function]
console.log(toString.call(undefined));  //[object Undefined]
console.log(toString.call(null)); //[object Null]
```

优点：精准判断数据类型

缺点：写法繁琐不容易记，推荐进行封装后使用

##### ⭐️  为什么要用call？

两点：

（1）改变this指向，指向我们判断的目标变量，因此用apply也是可以的

（2）因为每个数据的原始toString方法都将被改写，通过call，让该数据调用object上的toString方法，用以判断当前数据类型。

------

### ⭐️ 作用域和作用域链

**作用域：**规定变量和函数可作用的范围。

**作用域链：**每个函数都有一个作用域链，当查找变量或者函数时，会从局部作用域到全局作用域依次查找，这个作用域的集合就是作用域链。

------

### ⭐️ 闭包？常见的闭包有哪些？引起内存泄露？

#### **闭包：**

变量的作用域属于函数的作用域。当函数执行完成时，内存会被释放，但是闭包函数是函数内部的子函数，它可以访问上级的作用域，即使上级函数执行完作用域也不会被清除。简而言之**闭包就是可以访问另一个函数作用域中变量的函数。**

#### **常见的闭包：**

**ajax请求、定时器的回调，防抖和节流，函数柯里化**

#### **内存泄漏：**

什么是？内存泄漏是指不再用的内存没有被及时的释放出来，导致该内存段无法继续被使用。

策略？标记清除法和引用计数法

（1）全局变量【严格模式】；（2）被遗忘的定时器和回调函数【用完清除】；（3）闭包；（4）DOM的引用【赋null】

------

### ⭐️函数柯里化

> 多个参数的函数变换成1个单一参数的函数，并且转换成一个函数，该函数可以接受小于原始函数参数个数，并且等到累计参数个数与原始函数接受参数个数一致时，返回结果。

**核心：函数里面返回函数，做到参数复用**

场景：（1）参数复用（2）延时执行（3）提前确认

------

### ⭐️讲一讲promise

- promise是一种异步编程解决方案，解决了传统的解决方案回调函数中的**回调地狱**的问题，并且更加灵活易懂
- promise实例具有三种状态：pending，fufilled，rejected三种状态
  - 但是变化途径只有两种：pending---fufilled 或 pending --- rejected，状态一旦变化就凝固了。
- new promise（）接收一个函数，入参为resolve和reject
  - resolve（）进入 . then也就是执行成功的回调 ； reject（）进入 . catch失败的回调手动的捕获异常

- Promise对象方法有：
  - Promise.resolve：将现有对象转化为promise对象
  - Promise.reject：返回一个promise实例，该实例状态为rejected
  - ⭐️【手写题】Promise.all：将多个promise实例包装成1个新的promise实例
    - 其状态由all里面的promise实例决定，都为fulfilled才会变成fulfilled；只要有一个rejected，其状态就会变成rejected，并且返回第一个被rejected实例的返回值

#### 延伸问题：async/await

promise搭配async/await可以讲promise链式调用变为类似同步代码的形式。async/await必须在函数中使用，在函数钱加上async，await关键字后面是一个promise对象，并且返回该对象的值，看起来代码结构更加清晰。

------

###  == 和 ===区别是什么？

- ==是基于类型转换的相等，也就是先类型转换在比较

```js
null == undefined // true
1 == '1' //true
true == 1 // true
true == '1' //true
```

- ===是严格相等，不做任何的类型转换

------

### new运算符的实现机制

1.  首先创建了一个新的`空对象`
1.  `设置原型`，将对象的原型设置为函数的`prototype`对象。
1.  让函数的`this`指向这个对象，执行构造函数的代码（为这个新对象添加属性）
1.  判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。

------

### ⭐️【腾讯】怎么判断质数？

1. 判断是否为number类型，是否是整数，是否小于2，满足其一返回false
2. === 2时，返回true
3. 对n开算数平方根，此时算数平方根处于3-9，for循环遍历，如果n%i==0返回false，都不是就返回true
