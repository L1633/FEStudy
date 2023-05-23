<!--
 * @Author: hcs
 * @Date: 2023-02-08 15:08:01
 * @LastEditTime: 2023-04-17 11:07:57
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\网络相关知识\HTTP1.0和1.1和2.0区别.md
-->
## http 1.0 http 1.1 和 http 2.0




HTTP 1.0 最基础的协议 支持基本的 GET POST 方法

HTTP 1.1 缓存策略 cache-control E-tag 等
支持长连接 Connecting： keep-alive ,  一次 TCP 连接多次请求
断电续传，状态码 206
支持新的方法 PUT DELETE 等, 可用于 RESTFUL API


HTTP/2 
 头部压缩 
 在客户端和服务器两端建立“字典”，用索引号表示重复的字符串，还釆用哈夫曼编码来压缩整数和字符串，可以达到 50%~90% 的高压缩率。

需要客户端和服务器各自维护一份“索引表”，也可以说是 字典，压缩和解压缩就是查表和更新表的操作。

将 http1 中 纯文本形式的报文 全部采用二进制格式，

同一个消息往返的帧会分配一个唯一的流 ID。你可以想象把它成是一个虚拟的“数据流”，在里面流动的是一串有先后顺序的数据帧，这些数据帧按照次序组装起来就是 HTTP/1 里的请求报文和响应报文。
