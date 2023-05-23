<!--
 * @Author: hcs
 * @Date: 2023-02-08 15:09:58
 * @LastEditTime: 2023-04-17 13:32:33
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\网络相关知识\WebSocket和HTTP协议.md
-->
### WebSocket
  支持端对端通讯
  可以由 client 发起，也可以由 server 发起
  用于 消息通知，直播间讨论区，聊天室，协同编辑

  连接过程： 先发起一个 HTTP 请求
  成功之后再升级到 WebSocket 协议， 再通讯

  WebSocket 协议名是ws:// , 可双端发起请求
  WebSocket 没有跨域限制
  通过 send 和 onmessage 通讯 ( HTTP 通过 req 和 res)


### WebSocket和HTTP长轮询的区别
HTTP 长轮询： 客户端发起请求，服务端阻塞，不会立即返回
WebSocket: 客户端发起请求， 服务端也可以发起请求



