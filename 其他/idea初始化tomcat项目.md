<!--
 * @Author: hcs
 * @Date: 2023-03-23 18:52:50
 * @LastEditTime: 2023-04-17 13:41:34
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\其他\idea初始化tomcat项目.md
-->
<!--
 * @Author: hcs
 * @Date: 2023-03-23 18:52:50
 * @LastEditTime: 2023-03-27 11:03:06
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\其他\idea初始化tomcat项目.md
-->
## IDEA 社区版创建 web 项目
https://www.cnblogs.com/yjh1995/p/13894961.html

tomcat 相当于是 node 环境


### Servlet
接受单个参数
request.getParameter(key) 接受单个参数
String[] key =  request.getParameterValues(key) 接受多个同名参数
比如 url 上 ?spec=XXX & spec= YYY 同名的参数

Servlet 生命周期
  装载 - web.xml
  创建 - 构造函数
  初始化 - init()
  提供服务- service()
  销毁 - destroy()

```
public class FirstServlet extends HttpServlet {
    public FirstServlet() {
        System.out.println("创建");
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println("初始化");
    }

    // service 是请求处理的核心方法，无论是 get 或者 post 都会被 service() 处理
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      
    }

    @Override
    public void destroy() {
        System.out.println("销毁");
    }

}

```

注解 

Servlet 3.X 之后 引入了 '注解 Annotation' 特性

注解用于简化 Web 应用程序的配置过程

Servlet 核心注解： @WebServlet('/url')

就不在再 web.xml 中 配置了

启动时加载 Servlet

web.xml 使用 <load-on-startup> 设置启动加载

<load-on-startup>0-9999</load-on-startup>
0 优先级最高

启动时加载在工作中常用于系统的预处理（将一些耗时操作提前加载，使用户在使用的时候更流程）

注解中配置
@WebServlet(urlPatterns ="/unused",loadOnStartup =2)

通过 注解 ，如果不需要提供服务 ，但是 urlPatterns  这个是必须写的，是一个坑；随便写一个与其他不冲突的 地址就行了


servlet 获取请求头
request.getHeader(key)


```
请求转发
request.getRequestDispatcher(url).forward(request, response)
浏览器上的url 地址还是 第一次的 url
是服务器跳转， 只会产生一次请求

响应重定向
response.sendRedirect()
重定向是浏览器跳转， 会产生两次请求
```

Session 用于 保存于 浏览器窗口对应的数据

session 与 `浏览器窗口绑定`， 并且 存储在 tomcat 内存中


### ServletContext

就是一个全局对象， 所有的 servlet 都能读取 里面设置的变量

可以把一些 配置项设置 到这个 全局对象中

request.getServletContext()  // 获取

### FastJSON
Java 中 对象转 JSON, 使用 阿里巴巴的 FastJSON 库

序列化
  将 JavaBean 对象转换成 JSON
  JSON.toJSONString() , 可以对 obj， array ，map 等 进行 转换

反序列化
JSON.parseObject(json, Java Bean.class)
JSON.parseArray(json, Java Bean.class)

### 过滤器

跟 node 中 的 中间件一样
注意 在 doFliter 中一定要写 chain.deFilter(request, response), 使其能接着往下运行；
相当于 nodejs 中的 next

过滤器跟 koa 的 洋葱圈模型 一直， 根据  chain.deFilter 的 书写位置 来决定是 先走 下一个 过滤器还是 执行 当前过滤器的代码


过滤器对象 在 Web 应用启动时候被创建并且`全局唯一`， 在启动时候执行 init 方法， 在服务关闭的时候执行 destroy 方法， 在请求的时候 执行 对应的 doFileter 方法

`唯一` 的过滤器对象在并发环境中采用  `多线程` 提供服务


ServletRequest 接口 是最顶层的 接口， HttpServletRequest 接口 继承了 ServletRequest

ServletRequest 和 HttpServletRequest 接口是 javeEE 提供的，
而 tomcat 中 是通过 RequestFacade 类来实现的， 如果是其他的 服务， 提供的可能就不是这个类的， 它们会自己实现一个类；

通过注解形式的 filter， 按照类名的 升序排列 决定 执行顺序；
而 根据 web.xml 配置的 filter, 按照 <filter-mapping> 顺序执行， 在多个 filter 的情况下，推荐 通过 web.xml 配置；

### 监听器
跟 Vue 中的 watcher 功能类似

三种监听对象
1 ServlerContext - 对全局 ServlerContext 及其属性进行监听
2 HttpSession - 对用户会话及其属性操作进行监听
3 ServletRequest - 对请求及属性操作进行监听

过滤器与监听器的区别
◆ 过滤器(Filter)的职责是对URL进行过滤拦截，是主动的执行
◆ 监听器(Listener)的职责是对Web对象进行监听，是被动触发


开发监听器三要素
◆ 实现XxxListener接口，不同接口对应不同监听对象
◆ 实现每个接口中独有的方法，实现触发监听的后续操作
◆ 在web.xml中配置<listener>使监听器生效

6 种常用监听接口

内置对象监听接口
◆ServletContextListener-监听ServletContexti对象创建、销毁等操作
◆HttpSessionListener-监听HttpSession对象创建、销毁等操作
◆ServletRequestListener-监听HttpServletRequesti对象创建、销毁等



属性监听接口
◆ServletContextAttributeListener-监听全局属性操作
◆HttpSessionAttributeListener-监听用户会话属性操作
◆ServletRequestAttributeListener-监听请求属性操作

属性监听 用途比较少


MVC

Model 模型

模型负责生产业务需要的数据， 一般这个类以 Service 结尾， 比如 XxxService

Controller 控制器
控制器 Controller 是界面 View 和 模型 Model 粘合剂
一般就是 java 中的 Servlet

View 视图
视图 View 用于展示最终结果