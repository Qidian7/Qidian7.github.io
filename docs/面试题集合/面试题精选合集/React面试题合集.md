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

##### 2.6 useMemo/useCallback

> useMemo 用来缓存一个值，只有在依赖发生变化时才去重新计算这个值
>
> useCallback 用来缓存一个函数，只有在依赖发生变化时才去重新更新函数

##### 2.7 useId

> 在SSR中，客户端生成的随机id和服务器生成的随机id不一致

在CSR中id是稳定的，没有什么问题

在SSR中，会先在服务器随机生成一个id，然后以html的形式传给客户端，客户端在hydration的阶段再次生成随机id，就会导致服务器端和客户端id不一致

`const id = useId()`并且要注意，useId生成的id是带冒号的

##### 2.8 useTransition

> React18提出并发的概念，实现协调可中断，极大的提升用户的体验，用于区分紧急和非紧急更新。

------

### React生命周期

![react生命周期(新)](assert/react生命周期(新).png)

相比于旧的生命周期钩子，新钩子废弃了3个分别是：componentWillMount，componentWillReceiveProps和componentWillUpdate

新增了两个生命周期钩子：getDerivedStateFromProps和getSnapshotBeforeUpdate
