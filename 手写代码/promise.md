<!--
 * @Author: hcs
 * @Date: 2023-04-18 10:42:25
 * @LastEditTime: 2023-04-22 15:39:13
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\手写代码\promise.md
-->
## promise
使用
new Promise((resolve, reject) => {

}).then(res=> {
  
})

简易版本
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

function Mypromise (executor){
  this.state = PENDING;
  this.value = null;
  this.reason = null;
  this.onResolvedCallbacks = [];
  this.onRejectedCallbacks = []

  function resolve(value) {
    if (this.state === PENDING) {
      this.value = value;
      this.state = FULFILLED;
      this.onResolvedCallbacks.map(cb => cb(value));
    }
  }
  function reject(reason) {
    if (this.state === PENDING) {
      this.reason = reason;
      this.state = REJECTED;
      this.onRejectedCallbacks.map(cb => cb(reason))
    }
  }

  try {
    excutor(resolve, reject)
  } catch(e) {
    reject(e)
  }
}


Mypromise.prototype.then = function(onFulfilled, onRejected) {
  onFulfilled = typeof onFulfilled === "function" ? onFulfilled : value => value;
  onRejected = typeof onRejected === "function" ? onRejected : reason => reason;

  if (this.state === PENDING) {
    this.onResolvedCallbacks.push(onFulfilled);
    this.onRejectedCallbacks.push(onRejected);
  }

  if (this.state === FULFILLED) {
    onFulfilled(this.value);
  }

  if (this.state === REJECTED) {
    onRejected(this.reason);
  }
}

MyPromise.prototype.catch = function (onRejected) {
  return this.then(null, onRejected);
}

MyPromise.prototype.race = function(promises) {
  return new MyPromise((resolve, reject) => {
    for(let i = 0; i < promises.length; i++) {
      promises[i].then(resolve, reject);
    }
  })
}

MyPromise.prototype.all = function(promises) {
  let res = [];
  let count = 0;
  let len = promises.length;

  return new MyPromise((resolve, reject) => {
    for(let i = 0; i < promises.length; i++) {
      promises[i].then(data => {
        res[i] = data;
        count++;
        if (count === len) {
          resolve(res)
        }
      }, reject)
    }
  })
}

MyPromise.prototype.retry = function(fn , times, delay) {
  return new Promise((resovle, reject) => {
    
    funtcion retry() {
      fn.then(resolve).catch(e => {
        if(times === 0) {
          reject(e)
        } else {
          times--;
          setTimeout(retry, delay)
        }
      })
    }
    retry()
  })
}


class Scheduler {
  constructor() {
    this.queue = [];
    this.maxCount = 2;
    this.runCounts = 0;
  }
  add(promiseCreator) {
    this.queue.push(promiseCreator);
  }
  taskStart() {
    for (let i = 0; i < this.maxCount; i++) {
      this.request();
    }
  }
  request() {
    if (!this.queue || !this.queue.length || this.runCounts >= this.maxCount) {
      return;
    }
    // 更新当前执行的数量
    this.runCounts++;
    // 执行每一个 promise ，然后在它返回结果后更新  runCounts
    this.queue.shift()().then(() => {
      this.runCounts--;
      this.request();  // 接着执行，并判断
    });
  }
}