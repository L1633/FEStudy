<!--
 * @Author: hcs
 * @Date: 2023-04-18 09:44:58
 * @LastEditTime: 2023-04-18 09:48:29
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\构建工具相关知识\Tree-Shaking.md
-->
## Tree-Shaking 

Tree-Shaking 是一种基于 ES Module 规范的 Dead Code Elimination 技术，它会在运行过程中静态分析模块之间的导入导出，确定 ESM 模块中哪些导出值未曾其它模块使用，并将其删除，以此实现打包产物的优化。

Webpack 中，Tree-shaking 的实现一是先「标记」出模块导出值中哪些没有被用过，二是使用 Terser(terser作为js代码压缩工具) 删掉这些没被用到的导出语句。标记过程大致可划分为三个步骤：

    Make 阶段，收集模块导出变量并记录到模块依赖关系图 ModuleGraph 变量中

    Seal 阶段，遍历 ModuleGraph 标记模块导出变量有没有被使用

    生成产物时，若变量没有被其它模块使用则删除对应的导出语句

综上所述，Webpack 中 Tree Shaking 的实现分为如下步骤：

1、在 FlagDependencyExportsPlugin 插件中根据模块的 dependencies 列表收集模块导出值，并记录到 ModuleGraph 体系的 exportsInfo 中

2、在 FlagDependencyUsagePlugin 插件中收集模块的导出值的使用情况，并记录到 exportInfo._usedInRuntime 集合中

3、在 HarmonyExportXXXDependency.Template.apply 方法中根据导出值的使用情况生成不同的导出语句

4、使用 DCE 工具删除 Dead Code，实现完整的树摇效果