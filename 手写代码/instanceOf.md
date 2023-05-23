<!--
 * @Author: hcs
 * @Date: 2023-04-18 16:43:12
 * @LastEditTime: 2023-04-18 16:43:26
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\手写代码\instanceOf.md
-->
## instanceOf
function myInstanceof(left, right) {
  let prototype = right.prototype;
  left = left.__proto__;
  while(true){
    if (left === null || left === undefined) return false;
    if (left === prototype) return true;
    left = left.__proto__;
  }
}
