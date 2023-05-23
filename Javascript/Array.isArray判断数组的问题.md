<!--
 * @Author: hcs
 * @Date: 2023-03-29 15:20:21
 * @LastEditTime: 2023-03-29 15:25:13
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\Javascript\Array.isArray判断数组的问题.md
-->
### 判断数据类型 为数组的几种方式

Object.prototype.toString.call() 、 instanceof 以及 Array.isArray()

Object.prototype.toString.call() // "[object Array]"

如果是 null 
Object.prototype.toString.call(null) // "[object Null]"

Object.prototype.toString.call(undefined) // "[object Undefined]"

常用于判断浏览器内置对象时



instanceof的内部机制是通过判断对象的原型链中是不是能找到类型的 prototype

使用 instanceof 判断一个对象是否为数组，instanceof 会判断这个对象的原型 链上是否会找到对应的 Array 的原型，找到返回 true，否则返回 false。

但 instanceof 只能用来判断对象类型，原始类型不可以。`并且所有对象类型 instanceof Object 都是 true`。 

[] instanceof Object; // true

当检测 Array 实例时，Array.isArray 优于 instanceof ，因为 Array.isArray 可以 `检测出 iframes`.

let  iframe = document.createElement('iframe'); 
document.body.appendChild(iframe);
let xArray = window.frames[window.frames.length-1].Array;
let arr = new xArray(1,2,3);

Array.isArray(arr); // true

arr instanceof Array; // false