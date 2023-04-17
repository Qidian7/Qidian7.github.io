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

     
