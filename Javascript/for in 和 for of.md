## for in 和 for of
  for in 遍历得到 key
  for of 遍历得到 value
  for-in可以遍历可枚举的属性，for-of遍历的是可迭代的

  两者适用于不同的数据类型
  遍历对象： for in 可以， for of 不行
  遍历 Map Set: for in 不行, for of 可以
  遍历 generator: for in 不行， for of 可以

  for in 用于可枚举数据， 如 对象、数组、字符串
  for of 用于可迭代数据， 如 数组、字符串、Map、set

  tips: for-of 循环不支持普通对象,但是如果你想迭代一个对象的属性,可以使用 for-in 循环或者 Object.keys()方法。

### for in 会遍历到原型上的方法
使用for-in循环，返回的是所有能够通过对象访问的、`可枚举的属性`，其中既包括存在于实例中的属性，又包括存在于原型中的属性。
一般我们在使用过程中会结合 hasOwnProperty

```
  let obj = {};
  for (let i of obj) {
    if (obj.hasOwnProperty(i)){

    }
  }
```

