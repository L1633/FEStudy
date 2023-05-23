# nextTick


## 总结
 nextTick 是要把执行的任务推入到一个队列中， 在下一个 tick 同步执行

 数据改变后触发渲染 watcher 的 update， 但是 watchers 的 flush 是 在 nextTick 后， 所以重新渲染是异步的