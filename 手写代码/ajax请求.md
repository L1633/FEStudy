## ajax
const xhr = new XMLHttpRequest();
//true是异步
xhr.open('GET', "/url", true); 
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4 ) {
    if (xhr.status = 200) {
      console.log(xhr.responseText)
    }
  }
}
xhr.send(null)
// POSt 请求
xhr.send(JSON.stringify(postDataObj));

xhr.readyState
0-(未初始化)还没有调用send0方法
1-(载入)已调用send0方法，正在发送请求
2-(载入完成)send0方法执行完成，已经接收到全部响应内容
3-(交互)正在解析响应内容
4~(完成)响应内容解析完成，可以在客户端调用


function ajax(url) {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url, true)
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
          resolve(JSON.parse(xhr.responseText))
        } else if (xhr.status === 404 || xhr.status === 500) {
          reject(new Error('404 not found'))
        }
      }
    }
    xhr.send(null)
  })

  return p
}