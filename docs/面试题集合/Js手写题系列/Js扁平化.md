# Js扁平化

## 一.数组扁平化--flat

### 1.1 基本使用

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

