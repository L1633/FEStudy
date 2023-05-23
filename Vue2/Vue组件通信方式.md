<!--
 * @Author: hcs
 * @Date: 2023-03-21 09:37:09
 * @LastEditTime: 2023-04-18 11:54:06
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\Vue2\Vue组件通信方式.md
-->

## Vue 组件通讯的方式

1 props 和 $emit
2 自定义事件  如 event bus
3 $attrs: 如果子组件 没有再 props 和 emits 中 声明父组件 传过来的一些 参数，可以通过 这个属性获取 遗漏的 属性
4 $parent
5 $refs
6 provide/inject
7 Vuex