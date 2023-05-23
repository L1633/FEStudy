# JS 如何检测 iterable（可迭代对象）

## Iterator

Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即for...of循环
默认的 Iterator 接口部署在数据结构的Symbol.iterator属性，或者说，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）

function isIterable(obj) {
  if (obj === null || obj === undefined) {
    return false;
  }

  return typeof obj[Symbol.iterator] === 'function';
} 


对象obj是可遍历的（iterable），因为具有Symbol.iterator属性;
执行这个属性，会`返回一个遍历器对象`。该对象的根本特征就是具有`next`方法。每次调用next方法，都会返回一个代表当前成员的`信息对象`，具有 `value` 和 `done` 两个属性。

遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

Iterator 的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是 ES6 创造了一种新的遍历命令for...of循环，Iterator 接口主要供for...of消费。

const obj = {
  [Symbol.iterator] : function () {
    // 返回一个遍历器对象
    return {
      // 该对象有 next 方法
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
}

原生具备 Iterator 接口的数据结构如下。

  Array
  Map
  Set
  String
  TypedArray
  函数的 arguments 对象
  NodeList 对象

  let arr = ['a', 'b', 'c'];
  let iter = arr[Symbol.iterator]();

  iter.next() // { value: 'a', done: false }
  iter.next() // { value: 'b', done: false }
  iter.next() // { value: 'c', done: false }
  iter.next() // { value: undefined, done: true }

### 参考
  阮一峰老师的ECMAScript 6 入门