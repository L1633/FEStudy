<!--
 * @Author: hcs
 * @Date: 2023-03-29 14:08:49
 * @LastEditTime: 2023-04-18 16:11:05
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\Javascript\Set、Map、WeakSet 和 WeakMap.md
-->
###
ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
Set函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。
const set = new Set([1, 2, 3, 4, 4]);


WeakSet 结构与 Set 类似，也是不重复的值的集合.但是，它与 Set 有两个区别。

首先，WeakSet 的成员只能是`对象`，而不能是其他类型的值。
其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。

因此，WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。


WeakSet 是一个构造函数，可以使用new命令，创建 WeakSet 数据结构。
const ws = new WeakSet();


Map
ES6 提供了 Map 数据结构, 它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，`Object 结构提供了“字符串—值”`的对应，`Map 结构提供了“值—值”的对应`，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

Map 转为对象
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap.entries()) {
    obj[k] = v;
  }
  return obj;
}

WeakMap
WeakMap结构与Map结构类似，也是用于生成键值对的集合。
WeakMap与Map的区别有两点。

首先，WeakMap只接受`对象`作为键名（null除外），不接受其他类型的值作为键名。
其次，WeakMap的键名所指向的对象，不计入垃圾回收机制。


插入性能

    向 Object 和 Map 中插入新键/值对的消耗大致相当，当涉及到大量的插入操作时，Map 的性能更佳

查找速度 

    如果代码涉及大量查找操作时，某些情况下选用 Object 性能更好些。

删除性能

    对于大多数浏览器引擎来说，Map的delete()操作都比插入和查找更快，代码中涉及到大量删除操作时，应该选择 Map