##  Object.create
function myCreate(obj) {
  function F() {}
  F.prototype = obj;
  return new F();
}

new Objecet 和 Object.create() 区别
{} 等同于 new Object(), 原型 Object.prototype

Object.create(null) 没有原型， Object.create({})指定原型