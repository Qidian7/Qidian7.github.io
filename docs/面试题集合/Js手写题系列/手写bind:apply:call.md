# 手写bind/call/apply

### 手写bind

```js
/*
    思路：
    （1）判断是否是函数调用，若非函数调用抛异常
    （2）返回函数
        判断函数的调用方式，是否是被new出来的
        new出来的话返回空对象，但是实例的__proto__指向_this的prototype
    完成函数柯里化：Array.prototype.slice.call()
*/
```

```js
Function.prototype.myBind = function (context) {
    // 判断是否是一个函数
    if (typeof this !== "function") {
        throw new TypeError("Not a Function")
    }
    // 保存调用bind的函数
    const _this = this
    // 保存参数
    const args = Array.prototype.slice.call(arguments, 1)
    // 返回一个函数
    return function F() {
        // 判断是不是new出来的
        if (this instanceof F) {
            // 如果是new出来的
            // 返回一个空对象，且使创建出来的实例的__proto__指向_this的prototype，且完成函数柯里化
            return new _this(...args, ...arguments)
        } else {
            // 如果不是new出来的改变this指向，且完成函数柯里化
            return _this.apply(context, args.concat(...arguments))
        }
    }
}
```

### 手写call

```markdown
思路：
1. 判断是否是函数调用，如果不是抛出异常
2. 通过新对象（context）调用函数：
    给context创建一个fn设置为需要调用的函数
    结束调用之后删除fn
```

```js
Function.prototype.myCall = function (context) {
    if (typeof this !== 'function') {
        throw new TypeError('Not a function')
    }
    context = context || window
    context.fn = this
    let args = Array.from(arguments).slice(1)
    let result = context.fn(...args)
    delete context.fn
    return result
}
```

```js
function fn() {
    console.log('@@@@', this); //@@@@ { fn: [Function: fn] }
}
fn.myCall({})
```

### 手写apply

```js
Function.prototype.myApply = function (context) {
    if (typeof this !== 'function') {
        throw new TypeError('Not a function')
    }
    context = context || window
    context.fn = this
    let result
    if (arguments[1]) {
        result = context.fn(...arguments[1])
    } else {
        result = context.fn()
    }
    delete context.fn
    return result
}
```

```js
//! ====================测试=======================
function fn() {
    console.log('%%%%', this); //%%% { fn: [Function: fn] }
}
fn.myApply({})
```

