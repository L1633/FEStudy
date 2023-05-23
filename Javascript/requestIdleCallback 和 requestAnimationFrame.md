## requestIdleCallback 和 requestAnimationFrame
requestIdleCallback常用于做一些低优先级的任务。当一帧的末尾有空闲时间，才会执行回调函数


###  requestIdleCallback 和 requestAnimationFrame 区别
requestAnimationFrame 每次渲染完成都会执行, 高优
requestIdleCallback 空闲时候菜执行， 低优

 
requestAnimationFrame 和 requestIdleCallback 是和 `宏任务性质` 一样的任务，只是他们的执行时机不同而已
一个task(宏任务)--队列中全部job(微任务)--requestAnimationFrame--浏览器重排/重绘--requestIdleCallback

requestAnimationFrame 回调的执行与task和microtask无关，而是与浏览器是否渲染相关联的。它是在浏览器渲染前，在微任务执行后执行。

requestIdleCallback 是在浏览器渲染后有空闲时间时执行，如果requestIdleCallback设置了第二个参数timeout，则会在超时后的下一帧强制执行。