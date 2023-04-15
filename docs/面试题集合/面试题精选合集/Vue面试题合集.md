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

