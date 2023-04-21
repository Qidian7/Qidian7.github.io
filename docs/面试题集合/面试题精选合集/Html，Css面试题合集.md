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

------

### - 渐进增强与优雅降级的理解及区别

**渐进增强（Progressive Enhancement）：**
一开始就**针对低版本浏览器进行构建页面**，完成基本的功能，然后再针对高级浏览器进行效果、交互、追加功能达到更好的体验。

**优雅降级（Graceful Degradation）：**
一开始就**构建站点的完整功能**，然后针对浏览器测试和修复。比如一开始使用 CSS3 的特性构建了一个应用，然后逐步针对各大浏览器进行 hack 使其可以在低版本浏览器上正常浏览。
**两者区别**
1、广义：
其实要定义一个基准线，在此之上的增强叫做渐进增强，在此之下的兼容叫优雅降级
2、狭义：
渐进增强一般说的是使用CSS3技术，在不影响老浏览器的正常显示与使用情形下来增强体验，而优雅降级则是体现html标签的语义，以便在js/css的加载失败/被禁用时，也不影响用户的相应功能。

```css
/* 例子 */
.transition { /*渐进增强写法*/
  -webkit-transition: all .5s;
     -moz-transition: all .5s;
       -o-transition: all .5s;
          transition: all .5s;
}
.transition { /*优雅降级写法*/
          transition: all .5s;
       -o-transition: all .5s;
     -moz-transition: all .5s;
  -webkit-transition: all .5s;
}
```
