<!--
 * @Author: hcs
 * @Date: 2023-04-18 09:42:12
 * @LastEditTime: 2023-04-18 09:43:48
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\构建工具相关知识\webpack externals 配置.md
-->
## webpack externals 配置
，如果我们想引用一个库，但是又不想让webpack打包，并且又不影响我们在程序中以CMD、AMD或者window/global全局等方式进行使用，那就可以通过配置externals。这个功能主要是用在创建一个库的时候用的，但是也可以在我们项目开发中充分使用。

假设：我们开发了一个自己的库，里面引用了lodash这个包，经过webpack打包的时候，发现如果把这个lodash包打入进去，打包文件就会非常大。那么我们就可以externals的方式引入。也就是说，自己的库本身不打包这个lodash，需要用户环境提供。


externals 配置选项提供了「从输出的 bundle 中排除依赖」的方法

    externals 配置选项提供了「从输出的 bundle 中排除依赖」的方法 

    简单来说就是 防止将某些 import 的包( package) 打包到 bundle 中，而是在运行时(runtime)再去从外部获取这些扩展依赖(external dependencies))。

    相反，所创建的 bundle 依赖于那些存在于用户环境(consumer's environment)中的依赖。此功能通常对 library 开发人员来说是最有用的，然而也会有各种各样的应用程序用到它。

    externals: {
  "lodash": {
        commonjs: "lodash",//如果我们的库运行在Node.js环境中，import _ from 'lodash'等价于const _ = require('lodash')
        commonjs2: "lodash",//同上
        amd: "lodash",//如果我们的库使用require.js等加载,等价于 define(["lodash"], factory);
        root: "_"//如果我们的库在浏览器中使用，需要提供一个全局的变量‘_’，等价于 var _ = (window._) or (_);
  }
}