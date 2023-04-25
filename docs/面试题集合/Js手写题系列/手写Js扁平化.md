# Js扁平化

## 一.数组扁平化--flat

```js
// js扁平化
// flat（）默认展开一层，Infinity完全展开
let arr = [1, 2, 3, [4, 5, [6, [7]]]];
console.log(arr.flat(Infinity)); //[1, 2, 3, 4,5, 6, 7]
console.log(arr.flat(1)); //[ 1, 2, 3, 4, 5, [ 6, [ 7 ] ] ]
console.log(arr.flat(2)); //[ 1, 2, 3, 4, 5, 6,[ 7 ]]
```

```js
Array.prototype.myFlat = function (deep) {
  let res = [];
  deep--;
  // this我们用的数组
  for (let i of this) {
    if (Array.isArray(i) && deep >= 0) {
      res = res.concat(i.myFlat(deep));
    } else {
      res.push(i);
    }
  }
  return res;
};
```

```js
// test
let arr = [1, 2, 3, [4, 5, [6, [7]]]];
console.log(arr.myFlat(Infinity)); //[1, 2, 3, 4,5, 6, 7]
console.log(arr.myFlat(1)); //[ 1, 2, 3, 4, 5, [ 6, [ 7 ] ] ]
console.log(arr.myFlat(2)); //[ 1, 2, 3, 4, 5, 6,[ 7 ]]
```

## 二.对象扁平化

```js
// Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时		返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）
var obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj));//[['foo','bar'],['baz','42']]
```

```js
const flattenObj = (obj)=>{
  let res = {}
  function flat(obj,preKey = ''){
    Object.entries(obj).forEach(([key,val])=>{
      let newKey = preKey?`${preKey}.${key}`:key
      // 深层嵌套
      if(val && typeof val == 'object'){
        val.flat(val,newKey)
      }else {
        res[newKey] = val
      }
    })
  }
  flat(obj)
  return res
}
```

```js
// test
let obj = {name:'John',age:30,adress:{city:'Urumqi',state:'Uru'}}
console.log(flattenObj(obj))
/*
	{
		'name':'John',
		'age':30,
		'adress.city':'Urumqi',
		'adress.state':'Uru'
	}
*/
```

