## 📒 Node.js面试题合集

### 什么是 Node.js 中的 process？它有哪些方法和应用场景？

1. 在 Node.js 中，process 是一个**全局**变量，它提供了与当前 Node.js 进程相关的信息和控制。process 对象是 EventEmitter 的一个实例，因此它可以使用 EventEmitter 的 API，例如注册事件监听器和触发事件。
2. process 对象的应用场景：
   - 监听进程退出事件，执行资源清理操作。
   - 通过 process.argv 读取命令行参数。
   - 通过 process.env 读取环境变量。(常用)
   - 通过 process.cwd 和 process.chdir 修改 Node.js 进程的工作目录。
   - 通过 process.pid 获取进程 ID。
   - 通过 process.nextTick 将某个操作放到下一个 tick 队列中，以实现异步执行。

------

### 你对Node.js 的理解？优缺点？应用场景？

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，它允许开发者使用 JavaScript 进行服务器端编程。Node.js 拥有**事件驱动**、**非阻塞I/O的特性**，能够处理**高并发**的请求，因此它被广泛应用于实时应用、Web应用和API的开发。

以下是 Node.js 的优缺点：

优点：

- 事件驱动和非阻塞I/O的特性能够处理高并发的请求，提高了程序的性能和响应速度；
- 使用 JavaScript 进行开发，具有丰富的开源模块和组件，可以大大提高开发效率；
- 支持跨平台，可以在 Windows、Linux、MacOS 等多个操作系统上运行；
- 可以进行快速原型开发和实时调试。

缺点：

- Node.js 对于计算密集型的任务和多线程编程支持不够好，适合于 I/O 密集型任务；
- 因为 Node.js 是基于事件驱动和回调机制的，开发时需要特别注意回调地狱和异步操作的异常处理，否则容易导致代码难以维护。

Node.js 的应用场景主要包括以下几个方面：

- Web 开发：使用 Node.js 可以快速搭建Web服务器，进行Web开发；
- 实时应用：Node.js 支持事件驱动和非阻塞I/O，可以用于实时数据传输和消息通信等领域；
- 命令行工具：Node.js 可以用于编写命令行工具和脚本；
- 微服务：Node.js 支持跨平台和轻量级开发，可以用于编写微服务。

**前端工作中经常用node.js做后端中间层**

总之，Node.js 具有很多优点，如高并发处理能力、跨平台性、丰富的开源组件等，但也需要注意其局限性，开发者需要在实践中根据具体需求合理选用技术。

------

### 什么是 npm？你用过哪些 npm 包？是否开发过自己的 npm 包？

1. npm是**nodejs的包管理工具**，可以方便地安装、升级、删除、发布和管理Node.js的包（也称为模块）。npm包括一个命令行客户端，用于从npm仓库中下载、安装和管理依赖项，以及一个在线的包注册表，用于存储和分享npm包。

2. express，axios，sass，nanoid，lodash。这些包为JavaScript开发提供了许多便利的功能和工具，使得开发人员可以更快、更高效地构建应用程序。

3. 开发npm包的流程如下：

   ​	（1）新建项目文件夹，npm init 生成package.json；

   ​	（2）开发功能模块，确保遵循模块化的原则，编写模块的导出和引入。

   ​	（3）编写详细的readme文件，供使用者阅读

​		   （4）确保项目中包含.gitignore和.npmignore文件，以排除不需要发布到npm的文件和文件夹。

​		   （5）在npm官网 https://www.npmjs.com/ 注册一个账号，然后在命令行中使用 `npm login` 命令登录。

​		   （6）使用npm publish命令发布包到npm仓库。

------

### - 什么是 Node.js 的事件循环机制？它是怎么实现的？

Node.js事件循环机制是Node.js异步非阻塞I/O的核心。事件循环允许 Node.js 可以在单个线程中处理高并发的请求，提高了程序的性能和响应能力。

1. Node.js 在启动时会初始化事件循环。
2. 执行输入的脚本，可能会注册各种事件（如I/O操作、定时器等）。
3. 事件循环开始运行，进入不同的阶段（Phases），如Timers、I/O callbacks、Idle/Prepare、Poll、Check和Close callbacks。每个阶段负责处理特定类型的事件。
4. 当事件队列中的事件被处理完毕，事件循环会检查是否还有待处理的事件或回调。如果没有，事件循环结束，程序退出；否则，事件循环继续运行，处理新的事件。

------

### Node.js 有哪些全局对象？它们分别有什么作用？

Node.js 中有一些全局对象，它们可以在任何模块中直接访问，无需进行导入。以下是一些常见的全局对象及其作用：

1. `global`：它是 Node.js 的全局命名空间，类似于浏览器环境中的 `window` 对象。在 `global` 对象上定义的属性和方法可以在任何地方访问。然而，在实际开发中，应避免在 `global` 对象上添加属性，以防止全局命名空间污染。
2. `process`：它是一个全局对象，提供了关于当前 Node.js 进程的信息和对其进行控制的方法。`process` 对象包含诸如环境变量、命令行参数、内存使用情况、当前工作目录等属性和方法。
3. `console`：它是一个全局对象，提供了用于输出信息和调试的方法，如 `console.log()`、`console.error()`、`console.warn()` 等。
4. `setTimeout` 和 `clearTimeout`：这两个方法用于设置和清除定时器。`setTimeout` 方法用于在指定的毫秒数后执行一个回调函数，`clearTimeout` 方法用于取消一个先前通过 `setTimeout` 设置的定时器。
5. `setInterval` 和 `clearInterval`：这两个方法用于设置和清除周期性定时器。`setInterval` 方法用于每隔指定的毫秒数执行一个回调函数，`clearInterval` 方法用于取消一个先前通过 `setInterval` 设置的定时器。
6. `setImmediate` 和 `clearImmediate`：这两个方法用于在当前事件循环结束时执行一个回调函数。`setImmediate` 方法会将回调函数添加到事件循环的队列末尾，以便在当前事件循环的所有 I/O 事件和定时器事件处理完毕后执行。`clearImmediate` 方法用于取消一个先前通过 `setImmediate` 设置的回调函数。
7. `Buffer`：它是一个全局的构造函数，用于处理二进制数据，如文件 I/O、网络 I/O 等。`Buffer` 提供了一系列方法来创建和操作字节缓冲区。
8. `require` 和 `module`：`require` 是一个全局函数，用于导入其他模块。`module` 是一个全局对象，表示当前模块。每个 Node.js 文件都是一个模块，模块可以导出函数、对象或值，以便其他模块使用。
