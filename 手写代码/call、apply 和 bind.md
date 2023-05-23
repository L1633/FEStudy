<!--
 * @Author: hcs
 * @Date: 2023-04-18 15:39:59
 * @LastEditTime: 2023-04-18 16:40:21
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\手写代码\call、apply 和 bind.md
-->
## call apply bind
function myCall(context, ...args) {
  if (typeof this !== 'function') {
    throw new TypeError('Error');
  }
  context = context || window;
  context.fn = this; // this 就是 调用的函数fn   fn.myCall(obj, arg1, arg2)
  let res = context.fn(...args);
  delete context.fn;
  return res;
}

// apply  区别就是 参数 apply 是数组 ， call 单个，可以传很多参数
function myApply(context, args){
  if(typeof this !== 'function') throw new TypeError('Error');
  context = context || window;
  context.fn = this;   // 重点！！！！！！！！！！！！
  // 执行函数
  let res = context.fn(...args);
  delete context.fn;
  return res;
}

call 的 性能比 apply 的好一些， 尤其是在 传递给函数的参数 超过 3 个的 时候， 在开发的时候，使用 call 多一点

function myBind(context, ...args) {
  if(typeof this !== 'function') throw new TypeError('Error');
  let self = this;
  return function F() {
     // 考虑 new 的情况
    if(this instanceof F) {
      return new self(...args, ...arguments)
    }
    return self.apply(context, [...args, ...arguments])
  }
}