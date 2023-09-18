## 📒 React面试题合集

### React.js是 MVVM 框架吗?

不是，**首先**React是facebook公司推出的框架专注于view层，React的专注的中心是component，也就是页面元素均可以被抽象成一个个的组件，其次，mvvm的核心是数据的双向绑定，而React是单向数据流，状态驱动视图。**最后**React整体而言是函数式的思想，将组件设计成纯组件，通过状态或逻辑传参，因此是单向数据流。

------

### ⭐️ React中的通信方式

- 父传子 （props）
- 子传父（父props一个function向子组件，子组件调用function时上传参数给父组件）
- 跨级
  1. props+回调的方式，在共同的公共组件中绑定一个回调函数，发送方通过调用回调函数将数据传至公共组件中，在公共组件中通过props将参数传给接收方
  2. context

- 非嵌套关系组件：（1）redux，mobx状态管理；2）发布订阅模式

------

### ⭐️ 聊聊react中class组件和函数组件的区别，再谈谈你用过的hooks

#### 1.区别

类式组件使用es6中class来定义的的组件。函数式组件接收一个props并返回一个react元素。自从16.8版本推出hooks，官方已经渐渐推荐开发者使用hooks进行开发。我谈谈我的理解：

- cc组件**复用**一些行为会比较困难。react中复用组件其实很容易，但在cc中复用比组件细粒度更小的行为就会显的非常困难，虽然有props，高阶组件可以在一定程度上缓解，但是层层嵌套又会出现嵌套地狱。
- cc组件中对于一些逻辑复杂的组件**可读性**很差。cc组件中，一个逻辑复杂且紧密的功能，不得不放在多个生命周期钩子中，本来相互关联相互对照的代码被迫拆分，而在不同生命周期钩子中不相关的代码又被放在一起，可读性较差。
- 对于开发者而言，fc组件更易上手，学习成本略低于cc组件。

#### 2.hooks

常用的hook函数有：useState,useReducer,useRef,useEffect,useLayoutEffect,useMemo,useCallback,useContext以及React18中新增的useId和useTransition

**在我们使用这些hooks函数时，react中的renderWithHooks根据不同的状态对hooks进行不同的初始化【HooksDispatcherOnxxx】，分别有mount（current为空），update（current不为空），render，外部调用hooks（报错状态）**

##### 2.1 useState()

> 传入初始化值放在缓存中并返回一个状态值以及更新状态的方法

`const [state,countState] = React.useState(initValue)`

##### 2.2 useReducer()

> 传入三个值：初始值，更新状态方法，第三个非必需若有则state进行惰性初始化【是一个function，usestate也有，惰性初始化是react一种优化方法】，返回状态以及更新状态的方法

`const [state,dispatch] = React.useState(initState,reducer,init)`

通过分析源码不难发现，useState和useReducer是非常接近的API，那么谈谈不同：

（1）useState只需要传递一个状态即可，useReducer还需要reducer对于惰性初始化不是必选的，使用上useState更友好

（2）但如果当我们需要描述很多状态又或者是一个数组/对象时，就需要不停的申明使用useState，因此我们通常认为useReducer是useState的替代方案，通常使用场景如下：

- 如果你的state是一个数组或者对象
- 如果你的state变化很复杂，经常一个操作需要修改很多state
- 如果你希望构建自动化测试用例来保证程序的稳定性
- 如果你需要在深层子组件里面去修改一些状态（关于这点我们下篇文章会详细介绍）
- 如果你用应用程序比较大，希望UI和业务能够分开维护

##### 2.3  useContext()

> 可以帮助我们跨越组件层级直接传递变量，实现数据共享。

使用方法：（1）createEffect创建context容器（2）使用useState或useReducer设置状态（3）<容器.Provider value={xxxx}>用context容器包裹目标组件，通过value穿参（4）子组件通过useContext（context容器）接收参数

##### 2.4 useRef()

> 一种接近于原生hook的方法，创建一个容器给元素打个标

在mount阶段时，将{current:initValue}挂载到hook.memoizedState上，ref.current获取和改变最新的值

##### 2.5 useEffect/useLayoutEffect

> useEffect【异步阻塞】函数会在浏览器完成布局与绘制之后执行；模拟的生命周期钩子
>
> useLayoutEffect【同步阻塞】在DOM变更后浏览器执行绘制前执行

```js
语法和说明: 
        React.useEffect(() => { 
          // 在此可以执行任何带副作用操作
          return () => { // 在组件卸载前执行,相当于componentWillUnmount
          // 在此做一些收尾工作, 比如清除定时器/取消订阅等
          }
        }, [stateValue]) 
        // 如果什么都不写就是监视所有人，相当于componentDidMount+componentDidUpdate
        // 如果指定的是[], 回调函数只会在第一次render()后执行，相当于componentDidMount
        // [xxx] 当xxx更新时执行
    
可以把 useEffect Hook 看做如下三个函数的组合
        componentDidMount()
        componentDidUpdate()
    	componentWillUnmount() 
牛客题：若想在DOM变更后浏览器执行绘制前执行，可以使用useLayoutEffect【同步阻塞】
```

##### 2.6 useMemo/useCallback

> useMemo 用来缓存一个值，只有在依赖发生变化时才去重新计算这个值
>
> useCallback 用来缓存一个函数，只有在依赖发生变化时才去重新更新函数

###### 2.6.1 为什么要用到useMemo

在React中，useMemo是一个用于性能优化的Hook函数。它的作用是缓存计算结果，只有当依赖数组中的变量发生变化时，useMemo才会重新计算缓存值。当组件重新渲染时，如果依赖数组中的变量没有发生变化，useMemo会直接返回之前缓存的值，避免在每次渲染时都重新计算，从而提高组件的渲染性能。

##### 2.7 useId

> 在SSR中，客户端生成的随机id和服务器生成的随机id不一致

在CSR中id是稳定的，没有什么问题

在SSR中，会先在服务器随机生成一个id，然后以html的形式传给客户端，客户端在hydration的阶段再次生成随机id，就会导致服务器端和客户端id不一致。

`const id = useId()`并且要注意，useId生成的id是带冒号的。

##### 2.8 useTransition

> React18提出并发的概念，实现协调可中断，极大的提升用户的体验，用于区分紧急和非紧急更新。

#### 3.为什么Hooks不能写在条件语句或循环语句里

在 React 中，每个组件都有自己的状态和生命周期。Hooks 的作用就是帮助我们管理组件的状态和生命周期，需要保证在每次组件渲染时，Hooks 的执行顺序都是一致的。

如果将 Hooks 写在循环或条件语句中，那么每次渲染时，Hooks 的执行顺序都可能会发生变化。这样会导致组件状态发生不可预测的变化，从而影响组件的正确性。

------

### 说一下 react-fiber

#### 1）背景

react-fiber 产生的根本原因，是`大量的同步计算任务阻塞了浏览器的 UI 渲染`。默认情况下，JS 运算、页面布局和页面绘制都是运行在浏览器的主线程当中，他们之间是互斥的关系。如果 JS 运算持续占用主线程，页面就没法得到及时的更新。当我们调用`setState`更新页面的时候，React 会遍历应用的所有节点，计算出差异，然后再更新 UI。如果页面元素很多，整个过程占用的时机就可能超过 16 毫秒，就容易出现掉帧的现象。

#### 2）实现原理

- react内部运转分三层：
  - Virtual DOM 层，描述页面长什么样。
  - Reconciler 层，负责调用组件生命周期方法，进行 Diff 运算等。
  - Renderer 层，根据不同的平台，渲染出相应的页面，比较常见的是 ReactDOM 和 ReactNative。

`Fiber 其实指的是一种数据结构，它可以用一个纯 JS 对象来表示`：

```
const fiber = {
    stateNode,    // 节点实例
    child,        // 子节点
    sibling,      // 兄弟节点
    return,       // 父节点
}
```

- 为了实现不卡顿，就需要有一个调度器 (Scheduler) 来进行任务分配。优先级高的任务（如键盘输入）可以打断优先级低的任务（如Diff）的执行，从而更快的生效。任务的优先级有六种：
  - synchronous，与之前的Stack Reconciler操作一样，同步执行
  - task，在next tick之前执行
  - animation，下一帧之前执行
  - high，在不久的将来立即执行
  - low，稍微延迟执行也没关系
  - offscreen，下一次render时或scroll时才执行
- Fiber Reconciler（react ）执行过程分为2个阶段：
  - 阶段一，生成 Fiber 树，得出需要更新的节点信息。这一步是一个渐进的过程，可以被打断。阶段一可被打断的特性，让优先级更高的任务先执行，从框架层面大大降低了页面掉帧的概率。
  - 阶段二，将需要更新的节点一次过批量更新，这个过程不能被打断。
- Fiber树：React 在 render 第一次渲染时，会通过 React.createElement 创建一颗 Element 树，可以称之为 Virtual DOM Tree，由于要记录上下文信息，加入了 Fiber，每一个 Element 会对应一个 Fiber Node，将 Fiber Node 链接起来的结构成为 Fiber Tree。Fiber Tree 一个重要的特点是链表结构，将递归遍历编程循环遍历，然后配合 requestIdleCallback API, 实现任务拆分、中断与恢复。

从Stack Reconciler到Fiber Reconciler，源码层面其实就是干了一件递归改循环的事情

传送门 ☞[# 深入了解 Fiber](https://juejin.cn/post/7002250258826657799)

------

### React生命周期

![react生命周期(新)](assert/react生命周期(新).png)

相比于旧的生命周期钩子，新钩子废弃了3个分别是：componentWillMount，componentWillReceiveProps和componentWillUpdate

新增了两个生命周期钩子：getDerivedStateFromProps和getSnapshotBeforeUpdate
