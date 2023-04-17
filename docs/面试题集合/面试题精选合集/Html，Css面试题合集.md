## 📒 HTML&CSS面试题合集

### - HTML5 新特性、语义化

#### 1.概念

```markdown
HTML5语义化是指使用合理的正确的标签来布局页面【正确的标签做正确的事情】
```

#### 2.语义化标签

```css
<nav></nav>,
<header></header>,
<footer></footer>,
<main></main>,
<section></section>,
<article></article>……
```

#### 3.语义化优点

```markdown
1.增强页面可读性与可维护性
2.即使在没有css的加持下，也能很好的呈现页面
3.代码清晰，利于阅读
4.利于开发和维护
5.利于seo，爬虫会根据不同标签来赋予不同的权重
```

------

### - HTML5新特性有哪些

- 语义化的标签：header，footer，nav，article，section……
- 音视频处理
- canvas/webgl
- web存储 localstorage/sessionstorage
- websocket
- history API
- 表单控件 calendar、date、time、email、url、search

------

### - CSS 选择器及优先级

#### 选择器：

- id选择器（#xxx）
- 类选择器（.xxx）
- 属性选择器(a[rel="external"])
- 伪类选择器（:hover）
- 通配符（*）
- 标签选择器（div，p，span……）
- 子选择器（ul>li）
- 后代选择器（ul,li）
- 相邻选择器（hi+p）

#### 优先级：

**！important>内联样式>id选择器>类选择器/属性选择器/伪类选择器>标签选择器>通配符选择器**

