<!--
 * @Author: hcs
 * @Date: 2023-02-08 15:00:48
 * @LastEditTime: 2023-04-17 09:30:52
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\网络相关知识\options请求.md
-->
## HTTP跨域时为何要发送 options 请求 

   这是浏览器对复杂跨域请求的一种处理方式,在真正发送请求之前,会先进行一次预请求,就是我们刚刚说到的参数为OPTIONS的第一次请求,他的作用是用于试探性的服务器响应是否正确,即是否能接受真正的请求,如果在options请求之后获取到的响应是拒绝性质的,例如500等http状态,那么它就会停止第二次的真正请求的访问

    有三种方式会导致这种现象:

    1.请求的方法不是GET/HEAD/POST（非简单请求 比如 PUT DELETE）

    2.POST 请求的Content-Type 并非 application/x-www-form-urlencoded, multipart/form-data, 或text/plain

    3.请求设置了自定义的header字段

    比如我的Content-Type设置为"application/json;charset=utf-8"并且自定义了header选项导致了这种情况

### 总结
  options 请求 是跨域请求之前的预检查

  浏览器自行发起的, 无需我们干预

  不会影响实际的功能
