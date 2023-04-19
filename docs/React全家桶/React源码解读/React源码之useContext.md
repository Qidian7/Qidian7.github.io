# React源码之useContext

## 1.前言

**FC中组件间的通信**

## 2.useReducer源码解读

### 2.1 入口

```js
const HooksDispatcherOnMount: Dispatcher = {
  useContext: readContext,
};
const HooksDispatcherOnUpdate: Dispatcher = {
  useContext: readContext,
};
```

我们看到 `HooksDispatcherOnUpdate` 和 `HooksDispatcherOnMount` 里，关于 `useContext` 的定义，都是 `readContext` 方法。也就是说，无论是更新还是挂载，`useContext` 最终执行的都是 `readContext` 方法。那么让我们来分析一下`readContext` 

### 2.2 readContext

首先我们需要知道 `useContext` 的作用就是返回 context 容器当前的值，因此 `readContext` 的主要作用就是绑定依赖，以及返回当前值

FC 通过 `readContext`，将函数组件的 `dependencies` 和当前 `context` 建立起关联，context 改变，将当前函数组件设置高优先级，促使其渲染。

```js
// packages/react-reconciler/src/ReactFiberNewContext.js
// readContext，readContextForConsumer  合并精简之后
export function readContext<T>(context: ReactContext<T>): T {
    // 适配多平台,这两个的值是一样的
  const value = isPrimaryRenderer
    ? context._currentValue
    : context._currentValue2;
  if (lastFullyObservedContext === context) {
  } else {
    // 新建 context 链表的节点，节点上存储着用户传递的 context 对象，和 value
    const contextItem = {
      context: ((context: any): ReactContext<mixed>),
      memoizedValue: value,
      next: null,
    };
    // fiber 和 context 建立连接
    if (lastContextDependency === null) {
      // 这是组件的第一个依赖项，创建一个新的 context 依赖列表
      lastContextDependency = contextItem;
      currentlyRenderingFiber.dependencies = {
        lanes: NoLanes,
        firstContext: contextItem,
      };
      if (enableLazyContextPropagation) {
        currentlyRenderingFiber.flags |= NeedsPropagation;
      }
    } else {
      // 添加新的依赖项
      lastContextDependency = lastContextDependency.next = contextItem;
    }
  }
  return value;
}
```

总结流程如下：

1. 首先会创建一个context 链表的节点 `contextItem` ，上述说到过 fiber 上会存在多个 `dependencies` ，它们以链表的形式联系到一起
2. 如果不存在最后一个 `lastContextDependency` ，那证明 `contextDependencies` 为空 ，那么会创建第一个 `dependency` ，
3. 如果存在最后一个 `dependency` ，那么 `contextItem` 会以链表形式保存，并变成最后一个 `lastContextDependency` 。
4. 最后返回 context 对象上取出来的 `value`

> <span style='color:red;font-weight:bold;'>重点：主要的工作就是将 Fiber 和 context 建立连接，如果已经存在 `dependencies` 那就直接以链表的形式连接到 next 指针上，不然就初始化为第一个 `dependencies`</span>

`useContext` 需要结合 `createContext` 使用才有意义，通过 `createContext` 来创建 `context`容器，`useContext` 来从顶层获取到 `context`，实现全局状态管理的一种方式，让我们来解析一下`createContext`

### 2.3 createContext阶段

```js
//  packages/react/src/ReactContext.js
export function createContext<T>(defaultValue: T): ReactContext<T> {
  const context: ReactContext<T> = {
    // ReactContext中的$$typeof是作为createElement中的属性type中的对象进行存储的
    $$typeof: REACT_CONTEXT_TYPE,
    _currentValue: defaultValue,
    _currentValue2: defaultValue,
    _threadCount: 0, // 追踪 context 的并发渲染器数量
    // These are circular
    Provider: (null: any), // 提供组件
    Consumer: (null: any),
    _defaultValue: (null: any),
    _globalName: (null: any),
  };
  // 给context对象添加 Provider 属性，并且 Provider 中的_context指向的是 context 对象
  context.Provider = {
    $$typeof: REACT_PROVIDER_TYPE,
    _context: context,
  };
    ...
  // 为了保证Consumer拿到最新的值，直接让Consumer=React.Context，
  context.Consumer = context;
  return context;
}
```

在 `createContext` 中，构建一个 `context` 对象，将传递进来的 `defaultValue` 赋值给 `context` 对象的 `_currentValue` 和 `_currentValue2` 属性， 并在 `context` 对象上定义了一个用来追踪 `context` 并发渲染器数量的 `_threadCount` 属性， 以及一个为 Consumer 组件提供 value 的 Provider 组件，和一个用于使用 context 的 Consumer组件。

>  _ currentValue 和 _ currentValue2 两个属性是为了适配不同的平台，如 Web端、移动端。这两个属性在 context 对象初始化时都会赋值为传入的 defaultValue 。 在 React 更新的过程中，会一直有一个叫做 valueCursor 的栈，这个栈可以帮助记录当前的 context，每次更新组件的时候，_ currentValue 和 _ currentValue2 都会被赋值为最新的value

如上可以很容易的看清楚 context 对象的本质，三个属性重要的属性

- `Provider` 本质上是一个 element 对象，REACT_PROVIDER_TYPE 类型
- `Consumer` 本质上也是一个 element 对象，REACT_CONTEXT_TYPE 类型
- `_currentValue` 这个用来保存传递给 Provider 的 value

`context` 对象构建好之后，就将当前的 context 对象分别挂载到 Provider 组件和 Consumer 组件上。

## 3.总结

`createContext` 就是创建了一个包含 `Provider` 和 `Consumer` 的对象，对比到使用中就是创建一个容器，然后容器.Provider包裹子组件通过value传值

`useContext` 就是建立 context 和 Fiber 的连接，然后返回 value，对比到使用中就是拿到传递的值

至此useContext源码解析结束🔚！

