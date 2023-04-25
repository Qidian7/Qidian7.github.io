## React源码之Component&PureComponent

### 1.学习契机

**学习react到类式组件时看到React.Component于是来学习源码弄懂其中原理**

### 2.什么是Component，PureComponent？

都是class方式定义的类,两者没有什么大的区别,只是PureComponent内部使用shouldComponentUpdate(nextProps,nextState)方法，通过浅比较（比较一层），来判断是否需要重新render()函数，**如果外面传入的props或者是state没有变化，则不会重新渲染，省去虚拟dom的生成和对比过程，从而提高性能。**

```js
/*
   Component 基类
   1.设置react的props，content,refs,updater等属性
   2.要知道class继承是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this
   class A extends React.Component{construcroe(props){super(props)}
*/
function Component(props, context, updater) {
    this.props = props; // 父传子
    this.context = context; // 爷传孙
    this.refs = emptyObject; // 子到父传递数据 节点实例挂载进来
    this.updater = updater || ReactNoopUpdateQueue; // 更新数据
  }
  Component.prototype.isReactComponent = {}; //给Component原型上添加属性

/*
	给Component原型上增加方法setStates，使用setState来改变Component类内部的变量 
	enqueueSetState：实现更新机制
	partialState：要更新的state，可以是object/function
*/
  Component.prototype.setState = function (partialState, callback) {
    !(
      typeof partialState === "object" ||
      typeof partialState === "function" ||
      partialState == null
    )
      ? invariant(
          false,
          "setState(...): takes an object of state variables to update or a function which returns an object of state variables."
        )
      : void 0; // undefined
    this.updater.enqueueSetState(this, partialState, callback, "setState");
  };

//在Component的深层次改变但是没有调用setState时 调用此方法 强制更新一次
  Component.prototype.forceUpdate = function (callback) {
    this.updater.enqueueForceUpdate(this, callback, "forceUpdate");
  };
```

```js
 // ComponentDummy的原型继承Component的原型 
	function ComponentDummy() {}
  ComponentDummy.prototype = Component.prototype;

  function PureComponent(props, context, updater) {
    this.props = props;
    this.context = context;
    this.refs = emptyObject;
    this.updater = updater || ReactNoopUpdateQueue;
  }
	/*
		不是直接继承Component 而是PureComponent.prototype继承Component的原型 ，这样不会继承Component上constrcutor方法
		减少内存使用
	*/ 
  var pureComponentPrototype = (PureComponent.prototype = new ComponentDummy());
	//实例沿着原型链向上查询，只要是自己继承的，都被认作自己的构造函数
  pureComponentPrototype.constructor = PureComponent;
  objectAssign(pureComponentPrototype, Component.prototype);
//添加了这个isPureReactComponen参数 来判断是Component还是PureComponent组件
  pureComponentPrototype.isPureReactComponent = true;
```

####  2.1 PureComponent如何更新？

***checkShouldComponentUpdate 方法就是检查是否需要更新，当实例中有 shouldComponentUpdate 方法就会返回我们自己判断的的值(boolean) ;如果没有就根据 shallowEqual函数判断前后状态是否发生改变***

```js
  function checkShouldComponentUpdate(
    workInProgress,
    ctor,
    oldProps,
    newProps,
    oldState,
    newState,
    nextContext
  ) {
    var instance = workInProgress.stateNode;
   //📓 如果实例使用了shouldComponentUpdate则返回调用它的结果
    if (typeof instance.shouldComponentUpdate === "function") {
      startPhaseTimer(workInProgress, "shouldComponentUpdate");
      var shouldUpdate = instance.shouldComponentUpdate(
        newProps,
        newState,
        nextContext
      );
      stopPhaseTimer();

      {
        !(shouldUpdate !== undefined)
          ? warningWithoutStack$1(
              false,
              "%s.shouldComponentUpdate(): Returned undefined instead of a " +
                "boolean value. Make sure to return true or false.",
              getComponentName(ctor) || "Component"
            )
          : void 0;
      }

      return shouldUpdate;
    }
	//📓 如果使用PureReactComponent的时候进行浅对比
    if (ctor.prototype && ctor.prototype.isPureReactComponent) {
      return (
        // 前后值不同shallowEqual为false，此处！为true
        !shallowEqual(oldProps, newProps) || !shallowEqual(oldState, newState)
      );
    }

    return true;
  }

```

#### 2.2 判断前后状态是否改变

```js
  function shallowEqual(objA, objB) {
    if (is(objA, objB)) {
      // 一样返回true
      return true;
    }
			// 不是对象或者null 返回false
    if (
      typeof objA !== "object" ||
      objA === null ||
      typeof objB !== "object" ||
      objB === null
    ) {
      return false;
    }

    var keysA = Object.keys(objA);
    var keysB = Object.keys(objB);
		
    // 属性名不一样多返回false
    if (keysA.length !== keysB.length) {
      return false;
    }

    // Test for A's keys different from B.
    // 属性名不一样返回false
    for (var i = 0; i < keysA.length; i++) {
      if (
        !hasOwnProperty$1.call(objB, keysA[i]) ||
        !is(objA[keysA[i]], objB[keysA[i]])
      ) {
        return false;
      }
    }

    return true;
  }

```

