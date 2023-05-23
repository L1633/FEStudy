<!--
 * @Author: hcs
 * @Date: 2023-04-17 14:21:51
 * @LastEditTime: 2023-04-22 10:28:49
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\优化相关\Vue 和 React 优化.md
-->
## Vue 优化
1 v-if和 v-show
  V-if 彻底销毁组件
  V-show 使用CSS隐藏组件
  大部分情况下使用 V-if 更好，不要过度优化

2 v-for 使用 key

3 使用 computed 缓存

4 keep-alive 缓存组件
  频繁切换的组件， 如 tabs
  不要乱用，缓存太多会占用内存，且不好 debug

5 异步组件
  针对体积较大的组件，如编辑器、复杂表格，复杂表单等
  拆包，需要时异步加载，不需要时不加载
  减少主包的体积，首页会加载更快

6 路由懒加载

6 服务端 SSR

### React 优化
1 模拟 v-show
{ !flag && <MyComponent>}
<MyComponent style={{display: flag? "block" : "none"}}>

2 循环中使用 key
3 Fragment 减少层纪
4 JSX 中不要定义函数
5 构造函数 bind this
6 shouldComponentUpdate
7 useMemo
8 SSR
9 异步组件
10 路由懒加载