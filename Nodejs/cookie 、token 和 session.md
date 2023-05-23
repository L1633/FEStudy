## cookie 和 session
  由于 HTTP 是 无状态的， 要想保存用户的信息， cookie 和 session 就 应运而生;

  服务端通过向客户端发送 cookie， 然后每次请求 客户端携带 cookie 来帮助始别身份；


###  cookie token 和 session
  cookie 用于登录验证， 存储用户标识（如 userID）;

  session 在服务端，存储用户详细信息 和 cookie 信息 一一对应;

  cookie 和 session 是常见登录验证解决方案;

  cookie 是 http 规范，token 是自定义传递;

  cookie 会默认被浏览器存储， 而 token 需要自己存储;

  token 默认没有跨域限制;

  session 用户信息存储在服务器，占用服务内存，硬件成本高， 多进程 多服务时 不好同步 （需要使用第三反缓存， 如 redis）

### JWT (JSON Web Token)
  前端发起登录，后端验证成功之后， 返回一个加密的 token;

  前端自行存储这个 token (包含了加密的用户信息);

  每次访问服务器接口， 都要带着 这个 token，作为用户信息;

  JWT 不占用服务端内存， 多进程 多服务 不受影响 ，但是 万一密钥被泄露， 则用户信息会泄露， 且 token 体积较大，增加请求的数据量
  
### SSO 单点登录
1 基于 cookie
  cookie 默认不可跨域共享，但是有些情况可设置为共享

  比如 主域名相同， 如 www.baidu.com 和 image.baidu.com

  设置 cookie domain 为 主域名 即可共享

2 OAuth2

  简化模式的 OAuth2
  1 用户访问子应用， 但没有权限， 重定向至授权页面，等待用户授权

  2 用户授权完成，授权服务器（Authorization Server）返回一个 AccessToken

  3 再次携带 AccessToken 请求子应用 成功登录

  