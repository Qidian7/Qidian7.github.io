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

#### 3. constructor

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

### ⭐️【小米】怎么判断质数？

1. 判断是否为number类型，是否是整数，是否小于2，满足其一返回false
2. === 2时，返回true
3. 对n开算数平方根，此时算数平方根处于3-9，for循环遍历，如果n%i==0返回false，都不是就返回true

------

### 谈谈oop的理解

js是面向对象语言，一切即可对象，很重要的一点，在实际编程中，可以只关注功能，不关注内部细节的编程思想，就是面向对象。抽象，封装，继承等特性。

------

### 了解哪些排序算法？并说一下快排原理

冒泡，快排，插入，选择，桶排，堆排……

![排序算法](assert/排序算法.png)

#### 快排原理：

1. 选择一个元素作为基准点
2. 排序数组，比基准小的放在左边，比基准大的，放在右边。分割结束后，基准插入到中间去。
3. 递归，将放在左边和右边的数组进行1和2的操作

```js
var quickSort = function (arr){
  if(arr.length <= 1){
    return arr
  }
  let pivotIndex = Math.floor(arr.length / 2)
  let pivot = arr.splice(pivotIndex,1)[0]
  let left = [] , right = []
  for(let i = 0; i<arr.length; i++){
    if(arr[i] < pivot){
      left.push(arr[i])
    }else{
      right.push(arr[i])
    }
  }
  return quickSort(left).concat(pivot,quickSort(right))
}
```

### 原型 && 原型链

‌ **原型:**  在 JS 中，每个函数对象都有一个`prototype` 属性，这个属性指向函数的原型也称原型对象。

- 原型上可以存放属性和方法，共享给实例对象使用
- 原型可以做继承

**原型链**：对象都有__proto__属性指向对象的原型（对象），同时原型又是对象也有__proto__属性，指向原型对象的对象。因此可以利用__proto__一直指向Object的原型对象上，而Object原型对象用Object.prototype.__ proto__ = null表示原型链顶端。如此形成了js的原型链继承。同时所有的js对象都有Object的基本防范

**特点:**  `JavaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

### 深浅拷贝

基本数据类型放在栈中，引用数据类型将真实数据放在堆中并在栈中存放该真实数据的指针，该指针指向堆中真实数据的起始地址。

**浅拷贝**只是复制了某对象的指针，并不复制对象本身，新旧对象还是共享同一内存，一变都变。但是**深拷贝**会另外创造一个一模一个的对象，新旧对象不共享内存，修改一个并不会影响另一个

### JS 中 this 的情况

1. 普通函数调用：通过函数名()直接调用：`this`指向`全局对象window`（注意let定义的变量不是window属性，只有window.xxx定义的才是。即let a =’aaa’; this.a是undefined）
2. 构造函数调用：函数作为构造函数，用new关键字调用时：`this`指向`新new出的对象`
3. 对象函数调用：通过对象.函数名()调用的：`this`指向`这个对象`
4. 箭头函数调用：箭头函数里面没有 this ，所以`永远是上层作用域this`（上下文）
5. apply和call调用：函数体内 this 的指向的是 call/apply 方法`第一个参数`，若为空默认是指向全局对象window。
6. 函数作为数组的一个元素，通过数组下标调用的：this指向这个数组
7. 函数作为window内置函数的回调函数调用：this指向window（如setInterval setTimeout 等）

### 什么是箭头函数？能使用 new 来创建箭头函数么？

1. 相比普通函数，箭头函数有**更加简洁的语法**。
2. 箭头函数不绑定this，会**捕获其所在上下文的this**，作为自己的this。

箭头函数的外层如果有普通函数，那么箭头函数的this就是这个外层的普通函数的this，箭头函数的外层如果没有普通函数，那么箭头函数的this就是全局变量. 下面这个例子是箭头函数的外层有普通函数。

```arcade
let obj = {
  fn:function(){
      console.log('我是普通函数',this === obj)   // true
      return ()=>{
          console.log('我是箭头函数',this === obj) // true
      }
  }
}
console.log(obj.fn()())
```

下面这个例子是箭头函数的外层没有普通函数。

```js
let obj = {
    fn:()=>{
        console.log(this === window);
    }
}
console.log(obj.fn())// true
```

3. 箭头函数没有constructor是匿名函数，不能作为构造函数，**不可以使用new命令**，否则后抛出错误。 需要注意的是，箭头函数不能用作构造函数，也就是不能通过 new 关键字来创建实例。因为箭头函数没有自己的 this，而是继承了外层作用域的 this。如果用 new 来创建实例，就会出现意料之外的结果。
4. `箭头函数不绑定Arguments 对象`。取而代之用rest参数...解决。由于 箭头函数没有自己的this指针，通过 call() 或 apply() 方法调用一个函数时，只能传递参数（不能绑定this），他们的第一个参数会被忽略。（这种现象对于bind方法同样成立）

参考：[箭头函数与普通函数的区别](https://www.cnblogs.com/biubiuxixiya/p/8610594.html)

### EventLoop 事件循环

`JS`是单线程的，为了防止一个函数执行时间过长阻塞后面的代码，所以会先将同步代码压入执行栈中，依次执行，将异步代码推入异步队列，异步队列又分为宏任务队列和微任务队列，因为宏任务队列的执行时间较长，所以微任务队列要优先于宏任务队列。微任务队列的代表就是，`Promise.then`，`MutationObserver`，宏任务的话就是`setImmediate setTimeout setInterval`

JS运行的环境。一般为浏览器或者Node。 在浏览器环境中，有JS 引擎线程和渲染线程，且两个线程互斥。 Node环境中，只有JS 线程。 不同环境执行机制有差异，不同任务进入不同Event Queue队列。 当主程结束，先执行准备好微任务，然后再执行准备好的宏任务，一个轮询结束。

#### **浏览器中的事件环（Event Loop)**

事件环的运行机制是，先会执行栈中的内容，栈中的内容执行后执行微任务，微任务清空后再执行宏任务，先取出一个宏任务，再去执行微任务，然后在取宏任务清微任务这样不停的循环。

- eventLoop 是由JS的宿主环境（浏览器）来实现的；

- 事件循环可以简单的描述为以下四个步骤:

  1.  函数入栈，当Stack中执行到异步任务的时候，就将他丢给WebAPIs,接着执行同步任务,直到Stack为空；
  1.  此期间WebAPIs完成这个事件，把回调函数放入队列中等待执行（微任务放到微任务队列，宏任务放到宏任务队列）
  1.  执行栈为空时，Event Loop把微任务队列执行清空；
  1.  微任务队列清空后，进入宏任务队列，取队列的第一项任务放入Stack(栈）中执行，执行完成后，查看微任务队列是否有任务，有的话，清空微任务队列。重复4，继续从宏任务中取任务执行，执行完成之后，继续清空微任务，如此反复循环，直至清空所有的任务。

  ![事件循环流程](assets/342e581223d2471d9484fc48beb9f8e1~tplv-k3u1fbpfcp-zoom-1.image)

- 浏览器中的任务源(task):

  -   `宏任务(macrotask)`：\
      宿主环境提供的，比如浏览器\
      ajax、setTimeout、setInterval、setTmmediate(只兼容ie)、script、requestAnimationFrame、messageChannel、UI渲染、一些浏览器api
  -   `微任务(microtask)`：\
      语言本身提供的，比如promise.then\
      then、queueMicrotask(基于then)、mutationObserver(浏览器提供)、messageChannel 、mutationObersve

传送门 ☞ [# 宏任务和微任务](https://juejin.cn/post/7001881781125251086)
