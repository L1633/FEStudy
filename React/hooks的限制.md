<!--
 * @Author: hcs
 * @Date: 2023-04-17 15:37:55
 * @LastEditTime: 2023-04-22 11:02:50
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\React\hooks的限制.md
-->
## Hooks 的限制
不能在 React 的循环、条件或嵌套函数中调用 Hook

因为 Hooks 的设计是基于数组实现。在调用时按顺序加入数组中，如果使用循环、条件或嵌套函数很有可能导致数组取值错位，执行错误的 Hook。当然，实质上 React 的源码里不是数组，是`链表`。


### 防范措施
只需要在 ESLint 中引入 eslint-plugin-react-hooks 完成自动化检查就可以了

