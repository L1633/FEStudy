## EventLoop
 JS 是单线程的 （无论在浏览器还是 nodejs）
 浏览器中 JS 执行和 DOM 渲染共用一个线程 （互斥）


### 基本概念
JavaScript 分为 同步任务 和 异步任务；

所有同步任务都在  main thread 主线程 上执行，形成一个 执行栈（execution context stack），所有的任务都会被放到执行栈等待主线程执行；

执行栈 采用的是 后进先出 的规则，当函数执行的时候，会被添加到栈的顶部，当执行栈执行完成后，就会从栈顶移出，直到栈内被清空；

主线程之外，存在一个任务队列（task queue），在走主流程的时候，如果碰到异步任务，那么就在 任务队列 中放置这个异步任务；

一旦 执行栈 中所有 同步任务执行完毕，系统就会读取 任务队列，看看里面存在哪些事件。那些对应的异步任务 在异步任务有了结果后，将注册的回调函数放入 任务队列 中等待主线程空闲的时候（调用栈被清空），被读取到栈内等待主线程的执行。

任务队列 Task Queue，即队列，是一种先进先出的一种数据结构。

### 宏任务和微任务

在 JavaScript 中，任务被分为两种，一种 宏任务（MacroTask），一种叫 微任务（MicroTask）。

MacroTask（宏任务）有：setTimeout、setInterval、setImmediate、I/O、UI Rendering

MicroTask（微任务）有：Process.nextTick（Node独有）、Promise、Object.observe(废弃)、MutationObserver

`微任务和宏任务是绑定的，每个宏任务在执行时，会创建自己的微任务队列。`

宏任务结束后，会执行渲染，然后执行下一个宏任务， 而微任务可以理解成在当前宏任务执行后立即执行的任务。


### 浏览器中的 EventLoop

每次单个宏任务 执行完毕后，检查 微任务 (microTask) 队列是否为空，如果不为空的话，会按照 先入先出 的规则全部执行完微任务(microTask)后，设置 微任务(microTask)队列为null，然后再执行 宏任务，如此循环。

sync函数在await之前的代码都是同步执行的，可以理解为await之前的代码属于new Promise时传入的代码，await之后的所有代码都是在Promise.then中的回调

宏任务 --> 微任务 --> 渲染 --> 宏任务 --> ......


### nodejs 中 宏任务
nodejs 的 event loop 分为 6 个阶段，每当进入某一个阶段的时候，都会从对应的回调队列中取出函数去执行。当队列为空或者执行的回调函数数量到达系统设定的阈值，就会进入下一阶段

1 Timers - setTimeout setInterval
2 I/O callbacks - 处理网络、流、TCP 的错误回调
3 Idle、prepare - 闲置状态（nodejs 内部使用）
4 poll 阶段：获取新的I/O事件, 适当的条件下node将阻塞在这里
5 check 阶段：执行 setImmediate() 的回调
6 close callbacks 阶段：执行 socket 的 close 事件回调

 一开始执行栈的同步任务（宏任务）执行完毕后（将异步任务放入对应的队列），再会先去执行微任务（这点跟浏览器端的一样， 这个过程中 先执行 process.nextTick， 在执行其他 微任务，process.nextTick 优先级较高），接着进入 event loop 的 六个阶段，循环执行。

`在Node.js中，microtask 会在事件循环的各个阶段之间执行，也就是一个阶段执行完毕，就会去执行microtask队列的任务.`


### 文章
https://www.jianshu.com/p/8437de64d79d