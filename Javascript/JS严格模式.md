## JS 严格模式
JavaScript 是一门弱类型语言, 相对 Java 而言 它的语法规则就宽松很多， 因此在开发过程中有时候会因这种宽松的代码导致很多不可预测的错误。
因此 ECAMscript5 提出了"严格模式"，处于严格模式下运行的 JavaScript 代码会遵循更加苛刻的条件.

### 开启严格模式
使用 `use strict` 关键词
1 整个文件开启严格模式 在脚本的第一行 加上关键词
2 单个函数开启  在 函数内的第一行 加上关键词

### 严格模式的特点
1 声明变量必须要有关键词
2 禁止使用 with
3 函数不能有重名参数
  ```
  function foo(x, x) {
    //  Duplicate parameter name not allowed in this context
  }
  ```
4 禁止使用 eval()

  ```
  'use strict'
  eval('var x  = 10;');

  console.log(x) //  x is not defined


  ```
5 禁止 this 指向 window
