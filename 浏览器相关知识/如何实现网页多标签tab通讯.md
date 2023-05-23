## 如何实现网页多标签tab通讯

1 WebSocket
 无跨域限制 需要服务端支持， 成本高

2 通过 localStorage 通讯
`同域` 的 A 和 B 两个页面会
A 页面设置 localStorage
B 页面可监听到 localStorage 值得修改

```

window.addEventListener('storage', event => {
    console.info('key', event.key)
    console.info('value', event.newValue)
})

```
3 通过 SharedWorker 通讯

  SharedWorker 是 WebWorker 得一种

  WebWorker 可以开启子进程执行 JS , 但是不能操作 DOM

  SharedWorker 可单独开启一个进程， 用于 `同域页面` 通讯

### 如何实现网页和iframe之间的通讯
  postMessage
  
  ```
    <body>
      <p>
          index page
          <button id="btn1">发送消息</button>
      </p>

      <iframe id="iframe1" src="./child.html"></iframe>

    </body>
  ```

  ```
    iframe 
    btn1.addEventListener('click', () => {
            console.info('child clicked')
            window.parent.postMessage('world', '*')
        })

        window.addEventListener('message', event => {
            console.info('origin', event.origin) // 判断 origin 的合法性
            console.info('child received', event.data)
        })


    father
       btn1.addEventListener('click', () => {
            console.info('index clicked')
            window.iframe1.contentWindow.postMessage('hello', '*')
        })

        window.addEventListener('message', event => {
            console.info('origin', event.origin) // 来源的域名
            console.info('index received', event.data)
        })

  ```

