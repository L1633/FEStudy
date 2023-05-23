## 为什么需要JavaScript引擎呢？

高级的编程语言都是需要转成最终的机器指令来执行的；

事实上我们编写的lavaScript:无论你交给浏览器或者Node执行，最后都是需要被CPU执行的；
但是CPU只认识自己的指令集，实际上是机器语言，才能被CPU所执行；
所以我们需要JavaScript引擎帮助我们将JavaScript代码翻译成CPU指令来执行；
比较常见的JavaScript引擎有哪些呢？
1 SpiderMonkey:第一款JavaScript引擎，由Brendan Eich开发（也就是lavaScript作者）；
2 Chakra:微软开发，用于IT浏览器；
3 JavaScriptCore:VebKit中的JavaScript引擎，Apple公司开发；
4 V8:Google:开发的强大JavaScript引擎，也帮助Chrome从众多浏览器中脱颖而出；

###

浏览器内核与 JS 的引擎
比如 webkit  由两部分组成
1 webCore 负责 HTML 解析、布局、渲染等等相关的工作
2 JavaScriptCore 解析、执行 JavaScript 代码

在小程序中 编写的 JS 代码 就是 被  JavaScriptCore  执行的

### 
我们经常会说：不同的浏览器有不同的内核组成
Gecko:早期被Netscape和Mozilla Firefoxi浏览器浏览器使用；
Trident:微软开发，被虹E4~IE11浏览器使用，但是Edge浏览器已经转向Blink；
Webkit:苹果基于KHTML开发、开源的，用于Safari,Google Chrome.之前也在使用；
Blink:是Vebkit的一个分支，Google:开发，目前应用于Google Chrome、Edge、Opera等；

事实上，我们经常说的浏览器内核指的是浏览器的排版引擎：
排版引擎layout engine
也称为浏览器引擎(browser engine)、页面渲染引擎(rendering engine)
或样版引擎。

###
JavaScript --> Parse --> AST 抽象语法树 --> Ignition --> byteCode 字节码 --> 运行结果
                                              |             |
                              收集信息，比如类型信息          
                                              |             |
                                          TurboFan      MachineCode 优化的机器码 -->运行结果

为什么不直接 将 JS 转为 机器码, 而是 字节码
因为 不同平台 对应的 机器码 不同, 而 字节码 是通用的
所以先转成 字节码,  然后再 到 机器码 运行；
对于 经常用的 函数之类的 JS 代码 ，就会在 Ignition 阶段收集信息， 转换为机器码，
这样 就不需要在 用的时候 去转换， 节约了时间 ，提高效率

###
V8引擎本身的源码非常复杂，大概有超过100w行C++代码，通过了解它的架构，我们可以知道它是如何对JavaScript执行的：
1 Parse模块会将avaScript代码转换成AST(抽象语法树)，这是因为解释器并不直接认识JavaScript代码；

如果函数没有被调用，那么是不会被转换成 AST 的；
  Parse的V8官方文档：https:lv8.dev/blog/scanner

2 Ignition是一个解释器，会将AST转换成ByteCode(字节码)》
  同时会收集TurboFan优化所需要的信息（比如函数参数的类型信息，有了类型才能进行真实的运算）；
  如果函数只调用一次，Ignition会执行解释执行ByteCode;
  Ignition的V8官方文档：https://v8.dev/blog/ignition-interpreter

3 TurboFan是一个编译器，可以将字节码编译为CPU可以直接执行的机器码；
  如果一个函数被多次调用，那么就会被标记为热点函数，那么就会经过TurboFa转换成优化的机器码，提高代码的执行性能：
  但是，机器码实际上也会被还原为ByteCode,这是因为如果后续执行函数的过程中，类型发生了变化（比如sum函数原来执行的是 number类型，后来执行变成了string类型），之前优化的机器码并不能正确的处理运算，就会逆向的转换成字节码；

TurboFan的V8官方文档：https:lw8.dey/blog/turbofan-jit


### Blink 内核
■那么我们的JavaScripti源码是如何被解析(Parse过程)的呢？
■Blink将源码交给V8引擎，Stream获取到源码并且进行编码转换；
■Scanners会进行词法分析（lexical analysis),词法分析会将代码转换成 tokens:

■接下来 tokens 会被转换成 AST树，经过Parsera和PreParser:
 Parser就是直接将tokens转成AST树架构；
口PreParser称之为预解析，为什么需要预解析呢？
√这是因为并不是所有的)avaScript代码，在一开始时就会被执行。那么对所有的)avaScript代码进行解析，必然会
影响网页的运行效率；
√所以V8引擎就实现了Lazy Parsing(延迟解析)的方案，它的作用是将不必要的函数进行预解析，也就是只解析暂
时需要的内容，而对函数的全量解析是在函数被调用时才会进行；
√比如我们在一个函数outerp内部定义了另外一个函数inner,那么inner函数就会进行预解析；
■生成AST树后，会被Ignition转成字节码(bytecode),之后的过程就是代码的执行过程（后续会详细分析）。