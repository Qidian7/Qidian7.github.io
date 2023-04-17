## 📒 TypeScript面试题合集

### - 什么的TypeScript？

```markdown
(1)TypeScript是JavaScript的加强版，它给JavaScript添加了可选的静态类型和基于类的面向对象编程，它拓展了JavaScript的语法。
(2)而且TypeScript不存在跟浏览器不兼容的问题，因为在编译时，它产生的都是JavaScript代码。
```

------

### - TypeScript 和 JavaScript 的区别是什么？

Typescript 是 JavaScript 的超集【**TS就是能够在拥有JS所有特性下，赋予代码能在编写阶段(而非浏览器或node这些runtime阶段)有更好预见性的强类型语言**】，可以被编译成 JavaScript 代码。 用 JavaScript 编写的合法代码，在 TypeScript 中依然有效。Typescript 是纯面向对象的编程语言，包含类和接口的概念。 程序员可以用它来编写面向对象的服务端或客户端程序，并将它们编译成 JavaScript 代码。
TypeScript 引入了很多面向对象程序设计的特征，包括：

- interfaces  接口

- classes  类
- enumerated types 枚举类型
- generics 泛型
- modules 模块

主要不同点如下：
（1）TS 是一种面向对象编程语言，而 JS 是一种脚本语言（尽管 JS 是基于对象的）。
（2）TS 支持可选参数， JS 则不支持该特性。
（3）TS 支持静态类型，JS 不支持。
（4）TS 支持接口，JS 不支持接口。

------

### - 为什么要用 TypeScript ？

- TS 在开发时就能给出编译错误， 而 JS 错误则需要在运行时才能暴露。
- 作为强类型语言，你可以明确知道数据的类型。代码可读性极强，几乎每个人都能理解。
- TS 非常流行，被很多业界大佬使用。像 Asana、Circle CI 和 Slack 这些公司都在用 TS。

------

### - TypeScript 和 JavaScript 哪个更好？

由于 TS 的先天优势，TS 越来越受欢迎。但是TS 最终不可能取代 JS，因为 JS 是 TS 的核心。选择 TypeScript 还是 JavaScript 要由开发者自己去做决定。如果你喜欢类型安全的语言，那么推荐你选择 TS。 如果你已经用 JS 好久了，你可以选择走出舒适区学习 TS，也可以选择坚持自己的强项，继续使用 JS。
