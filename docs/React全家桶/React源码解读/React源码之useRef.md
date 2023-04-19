# React源码之useRef

## 1.基本实用

```js
const myContainer = React.useRef(); 
  function showMessage() {
    alert(myContainer.current.value);
  }
  return (
    <div>
      {/* 函数式组件无this */}
      <input type="text" ref={myContainer} />
      <button onClick={showMessage}>点击提示input的信息</button>
    </div>
);
```

不难发现useRef和createRef()很像

回顾一下React.createRef调用后可以返回一个容器（堆内存中），该容器可以存储被ref所标识的节点,专人专用只能放一个



## 2.mountRef

```js
function mountRef<T>(initialValue: T): {|current: T|} {
  const hook = mountWorkInProgressHook();
  const ref = {current: initialValue};
  hook.memoizedState = ref;
  return ref;
}
```

mountRef 的实现十分简单。

1. 首先会创建一个 hook 对象，该 hook 对象将会被添加到 `workInProgressHook` 单向链表中，接下来将要创建的 ref 对象将会被缓存到该 hook 对象上

> mountWorkInProgressHook 和 updateWorkInProgressHook 这两个方法的详解可以在[这里](React全家桶/React源码解读/React源码之useState.md)看到 本部分不再做讲解

```js
const hook = mountWorkInProgressHook();
```

2. 接着创建一个 ref 对象，其 `current` 属性初始化为传入的参数(initialValue)：

```js
const ref = { current: initialValue };
```

3. 然后将 ref 对象缓存到 hook 对象的 `memoizedState` 属性上

```js
hook.memoizedState = ref;
```

4. 最后返回一个可变的 ref 对象，**其属性 current 发生变化时，不会引发组件重新渲染**

```js
return ref;
```

## 3.updateRef

调用 updateRef 函数，通过 `updateWorkInProgressHook` 方法直接取出 hook.memoizedState。

```js
function updateRef<T>(initialValue: T): {|current: T|} {
  const hook = updateWorkInProgressHook();
  return hook.memoizedState;
}
```



## 4. 总结

可以看到在 `mount` 时 `hook.memoizedState` 挂的就是一个对象 `{ current: initialValue }`，这就解释了我们为什么用的是`myRef.current`并且通过可以直接通过 `myRef.current` 去**改变和获取最新的值**，不必进行任何依赖注入,因为它的本质就是一个对象哈，并不会引发组件的渲染，影响其他的东西。

至此useRef源码解析结束🔚！
