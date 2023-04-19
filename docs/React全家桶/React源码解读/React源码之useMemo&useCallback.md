# React源码之useMemo & useCallback

## 1.学习契机

> 在做在线文档项目是用到了useMemo，查阅资料发现useMemo和useCallback功能类似，于是来探究其深层原理。



## 2.useMemo/useCallback 小结

```markdown
# useMemo 用来缓存一个值，只有在依赖发生变化时才去重新计算这个值
# useCallback 用来缓存一个函数，只有在依赖发生变化时才去重新更新函数
```



## 3.useMemo/useCallback源码解读

### 3.1 mount时

>  **mount时分别调用了mountCallback/mountMemo**

```js
const HooksDispatcherOnMount: Dispatcher = {
  useCallback: mountCallback,
  useMemo: mountMemo,
  …………
};
```

#### 3.1.1 mountMemo

```js
//! mount
function mountMemo<T>(
  nextCreate: () => T,
  deps: Array<mixed> | void | null
): T {
  const hook = mountWorkInProgressHook();
  const nextDeps = deps === undefined ? null : deps;
  //1 计算需要缓存的值
  const nextValue = nextCreate();
  //1 将计算后的缓存的值和对应的依赖项保存到hook对象的memoizedState上
  hook.memoizedState = [nextValue, nextDeps];
  return nextValue;
}
```

##### 3.1.1.1 mountWorkInProgressHook

```js
//1 创建一个新的hook对象并添加到hook链表中，返回当前的workInProgressHook
function mountWorkInProgressHook(): Hook {
  //1 创建一个hook对象
  const hook: Hook = {
    memoizedState: null,

    baseState: null,
    baseQueue: null,
    queue: null,

    next: null,
  };
  //1  workInProgressHook是一个全局变量，只有第一次打开页面时是null
  if (workInProgressHook === null) {
    //1 链表上的第一个hook
    //1 直接赋值给 currentlyRenderingFiber 上的 memoizedState
    currentlyRenderingFiber.memoizedState = workInProgressHook = hook;
  } else {
    //1 非空时那就接到 next 上面，形成链表
    workInProgressHook = workInProgressHook.next = hook;
  }
  return workInProgressHook;
}
```

#### 3.1.2  mountCallback

```js
function mountCallback<T>(callback: T, deps: Array<mixed> | void | null): T {
    //1 创建一个新的hook对象并添加到hook链表中，返回当前的workInProgressHook
    const hook = mountWorkInProgressHook();
    const nextDeps = deps === undefined ? null : deps;
    //1 与useMemo不同的是useCallback缓存的是一个回调函数
    hook.memoizedState = [callback, nextDeps];
    return callback;
}
```



### 3.2 Updata时

> Updata时分别调用了UpdataMemo/UpdataCallback

```js
const HooksDispatcherOnMount: Dispatcher = {
  useCallback: UpdataCallback,
  useMemo: UpdataMemo,
  …………
};
```

#### 3.2.1  UpdataMemo

```js
//! update
function updateMemo<T>(
  nextCreate: () => T,
  deps: Array<mixed> | void | null
): T {
  const hook = updateWorkInProgressHook();
  const nextDeps = deps === undefined ? null : deps;
  //1 mountMemo时存储在hook对象的memoizedState上的缓存值及其依赖项
  const prevState = hook.memoizedState;
  if (nextDeps !== null) {
    const prevDeps: Array<mixed> | null = prevState[1];
    //1 ReactFiberHooks中的一个函数用来判断是否相等，相等返回true，不相等返回false
    if (areHookInputsEqual(nextDeps, prevDeps)) {
      return prevState[0];
    }
  }
  //1 不相等时返回新的缓存值
  const nextValue = nextCreate();
  hook.memoizedState = [nextValue, nextDeps];
  return nextValue;
}
```

#### 3.2.2  UpdataCallback

```js
function updateCallback<T>(callback: T, deps: Array<mixed> | void | null): T {
    const hook = updateWorkInProgressHook();
    const nextDeps = deps === undefined ? null : deps;
    const prevState = hook.memoizedState;
    if (nextDeps !== null) {
        const prevDeps: Array<mixed> | null = prevState[1];
        if (areHookInputsEqual(nextDeps, prevDeps)) {
            return prevState[0];
        }
    }
    hook.memoizedState = [callback, nextDeps];
    return callback;
}
```









