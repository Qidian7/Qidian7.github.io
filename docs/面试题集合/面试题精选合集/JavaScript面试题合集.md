## 📒 JavaScript面试题合集

### - JS中的8种数据类型及区别

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

### - JS中的数据类型检测方案

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

#### 3. Object.prototype.toString

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

------

