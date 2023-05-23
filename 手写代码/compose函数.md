<!--
 * @Author: hcs
 * @Date: 2023-03-30 11:25:40
 * @LastEditTime: 2023-04-18 10:41:48
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\手写代码\compose函数.md
-->

### compose
compose 函数 就是 将后一个 函数的结果当作 前一个函数的参数 

// 就是 a(b((c(...args))) 
// compose在函数式编程中是一个很重要的工具函数，在这里实现的compose有三点说明
// 第一个函数是多元的（接受多个参数），后面的函数都是单元的（接受一个参数）
// 执行顺序的自右向左的
// 所有函数的执行都是同步的

function compose(...funcs) {
    if (funcs.length === 0) {
        return arg => arg
    }
    if (funcs.length === 1) {
        return funcs[0]
    }
    return funcs.reduce((a, b) => (...args) => a(b(...args)))  // 这里 return 的是一个函数
}
compose（a, b, c);    相当于 a(b(c()))