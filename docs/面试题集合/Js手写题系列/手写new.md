# 手写new

```js
/*
	 字面量创建对象和new创建的区别：
	 （1）字面量创建更简单，方便阅读；
	 （2）字面量创建的不需要作用域解析，速度较快
*/
```

```js
function myNew(fn,...args){
  // 1.新建一个空对象
  let obj = {}
  // 2.对象obj的原型对象指向fn的原型对象
  obj.__proto__ = fn.prototype 
  // 3.改变this指向(指向obj)，并且执行构造函数内部的代码（传参），结果保存为res
  let res = fn.apply(obj,args)
  // 4.判断结果类型
  return res instanceof Object ? result : obj
}
```

```js
// test
function one(name,age){
  this.name = name
  this.age = age
}
let a = new one ('tom',29)
let b = myNew(one,'lily' , 25)
console.log(a) // one {name:'tom',age:29}
console.log(b) // one {name:'lily',age:25}
```

