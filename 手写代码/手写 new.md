<!--
 * @Author: hcs
 * @Date: 2023-03-29 14:58:41
 * @LastEditTime: 2023-04-18 14:52:16
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\手写代码\手写 new.md
-->
### new 

function create() {
  // 获取构造函数 同时删除 arguments 中第一个参数
  let con = [].shift.call(arguments);
  // 创建一个空的对象并链接到原型，obj 可以访问构造函数原型中的属性
  let obj = Object.create(con.prototype);
  //绑定 this 实现继承，obj 可以访问到构造函数中的属性
  let res = con.apply(obj, arguments);
  //判断返回值是不是一个对象,如果是,那么返回这个对象,否则返回新创建的对象
  return typeof res === 'object' ? res : obj;
}

