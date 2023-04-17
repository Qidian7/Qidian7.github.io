## 📒 Vue面试题合集

### - 简述MVVM

#### 1.什么是mvvm？

```markdown
模型视图双向绑定，model-view-viewmodel，也就是把`MVC`中的`Controller`演变成`ViewModel。Model`层代表数据模型，`View`代表UI组件也就是模版，`ViewModel`是`View`和`Model`层的桥梁也就是我们通常所说的vue实例，model里面存放数据
```

![mvvm](assert/mvvm.jpg)

#### 2.mvvm的优点

```markdown
1.低耦合。view可以独立于model的变化和修改，model可以绑定在多个view上，当view变化时model不变，model变化时view也可以不变
2.可重用性高。
3.可独立开发。
4.可维护性
```

------

### - Vue底层实现原理

vue.js是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter和getter，在数据变动时发布消息给订阅者，触发相应的监听回调
Vue是一个典型的MVVM框架，模型（Model）只是普通的javascript对象，修改它则试图（View）会自动更新。这种设计让状态管理变得非常简单而直观

**Observer（数据监听器）** : Observer的核心是通过Object.defineProprtty()来监听数据的变动，这个函数内部可以定义setter和getter，每当数据发生变化，就会触发setter。这时候Observer就要通知订阅者，订阅者就是Watcher

**Watcher（订阅者）** : Watcher订阅者作为Observer和Compile之间通信的桥梁，主要做的事情是：

1.  在自身实例化时往属性订阅器(dep)里面添加自己
1.  自身必须有一个update()方法
1.  待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调

**Compile（指令解析器）** : Compile主要做的事情是解析模板指令，将模板中变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加鉴定数据的订阅者，一旦数据有变动，收到通知，更新试图
