## 手写instanceOf

```js
/*
	思路：顺着原型链一直往上找
*/
```



```js
function myInstanceOf(L,R){
  let RP = R.prototype
  let LP = L.__proto__
  while(true){
    if(LP === null){
      return false
    }
    if(LP = RP){
      return true
    }
    LP = LP.__proto__
  }
}
```



