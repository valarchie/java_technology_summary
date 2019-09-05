
# 1.启动项目时如何实现不在链接里输入项目名就能启动?
修改Tomcat配置文件 server.xml。将Context标签内的path属性改为“/”即可。

# 2.一分钟之内只能处理1000个请求，你怎么实现，手撕代码?
每处理完一个请求，就往Application作用域内的一个LinkedBlockingQueue中添加一个时间戳。
每接收到一个请求的时候，就先判断Application作用域当中的LinkedBlockingQueue当中的size是否大于1000，判断前先remove  
掉60秒之前的时间戳。


# 3.什么时候用assert？
在开发和测试过程中，很多时候我们需要对某些场景进行判断，当这个判断满足我们预期的答案，才继续往下执行，否则终止程序或者发出警告。  
比较常用在单元测试。

# 4.Java应用服务器有哪些？
WebLogic， WebSphere， Jboss， Tomcat

# 5.JSP内置对象及常用方法？

1. request对象。
```
resquest对象是javax.servlet.http.HttpServletRequest类的一个实例。客户端的请求信息封装在resquest中发送给服务器端。
request的作用域是一次请求。
请求方式：request.getMethod()
请求的资源：request.getRequestURI()
请求用的协议：request.getProtocol()
请求的文件名：request.getServletPath()
请求的服务器的IP：request.getServerName()
请求服务器的端口：request.getServerPort()
客户端IP地址：request.getRemoteAddr()
客户端主机名：request.getRemoteHost()
```

2. response对象。
```
response对象是javax.servlet.http.HttpServletResponse的一个实例。服务端的相应信息封装在response中返回。
重定向客户端请求 response.sendRedirect(index.jsp)
```

3. session对象。
```
session对象是javax.servlet.http.HttpSession的一个实例。在第一个JSP页面被装载时自动创建，完成会话期管理。
session对象内部使用Map类来保存数据，因此保存数据的格式为 “Key/value”。
获取Session对象编号 session.getId()
添加obj到Session对象 session.setAttribute(String key,Object obj)
获取Session值 session.getAttribute(String key)
```

4. application对象。
```
application对象是javax.servlet.ServletContext的一个实例。
实现了用户间数据的共享，可存放全局变量。它开始于服务器的启动，直到服务器的关闭，在此期间，此对象将一直存在。
添加obj到Application对象 application.setAttribute(String key,Object obj)
获取Application对象中的值 application.getAttribute(String key)
``` 

5. out 对象。
```
out对象是javax.servlet.jsp.jspWriter的一个实例。用于浏览器输出数据。
输出各种类型数据 out.print()
输出一个换行符 out.newLine()
关闭流 out.close()
```

6. pageContext 对象。
```
pageContext 对象是javax.servlet.jsp.PageContext的一个对象。作用是取得任何范围的参数，通过它可以获取 JSP页面的out、request、reponse、session、application 等对象。
```

7. config 对象。
```
config 对象是javax.servlet.ServletConfig的一个对象。主要作用是取得服务器的配置信息。通过 pageConext对象的
getServletConfig() 方法可以获取一个config对象。
```

8. cookie 对象。
```
cookie 对象是Web服务器保存在用户硬盘上的一段文本。唯一的记录了用户的访问信息。
将Cookie对象传送到客户端 Cookie c = new Cookie(username",john"); 
读取保存到客户端的Cookie response.addCookie(c)
```

9. exception 对象。
```
exception 对象的作用是显示异常信息，只有在包含 isErrorPage="true" 的页面中才可以被使用。
```



# 6.JSP 和 Servlet 有哪些相同点和不同点，他们之间的联系是什么？
    jsp和servlet的区别和联系：
    1.jsp经编译后就变成了Servlet.(JSP的本质就是Servlet，JVM只能识别java的类，不能识别JSP的代码,Web容器将JSP的代码编译成
    JVM能够识别的java类)
    2.jsp更擅长表现于页面显示,servlet更擅长于逻辑控制.
    3.Servlet中没有内置对象，Jsp中的内置对象都是必须通过HttpServletRequest对象，HttpServletResponse对象以及HttpServlet  
    对象得到.
    Jsp是Servlet的一种简化，使用Jsp只需要完成程序员需要输出到客户端的内容，Jsp中的Java脚本如何镶嵌到一个类中，由Jsp容器
    完成。而Servlet则是个完整的Java类，这个类的Service方法用于生成对客户端的响应。
    联系：JSP是Servlet技术的扩展，本质上就是Servlet的简易方式。JSP编译后是“类servlet”。Servlet和JSP最主要的不同点在于，
    Servlet的应用逻辑是在Java文件中，并且完全从表示层中的HTML里分离开来。而JSP的情况是Java和HTML可以组合成一个扩展名为
    .jsp的文件。JSP侧重于视图，Servlet主要用于控制逻辑
    
# 7.四种会话跟踪技术

1. 隐藏表单域：<input type="hidden">，非常适合步需要大量数据存储的会话应用。
2. URL重写: URL可以在后面附加参数抛回给客户端，客户端在下一次请求携带回给服务器。
3. Cookie:一个 Cookie 是一个小的，已命名数据元素。服务器使用 SET-Cookie 头标将它作为 HTTP
响应的一部分传送到客户端，客户端被请求保存 Cookie 值，在对同一服务器的后续请求使用一个
Cookie 头标将之返回到服务器。与其它技术比较，Cookie 的一个优点是在浏览器会话结束后，甚至
在客户端计算机重启后它仍可以保留其值。
4. Session：使用 setAttribute(String str,Object obj)方法将对象捆绑到一个会话

# 8.说说weblogic中一个Domain的缺省目录结构?比如要将一个简单的helloWorld.jsp放入何目录下,然后在浏览器上就可打入主机？
Domain目录\服务器目录\applications，将应用目录放在此目录下将可以作为应用访问，如果是web应用，应用目录需要满足Web应用  
目录要求，jsp文件可以直接放在应用目录中，JavaBean需要放在应用目录的WEB-INF目录的classes目录中，设置服务器的缺省应用将  
可以实现在浏览器上无需输入应用名。

# 9.jsp有哪些动作?作用分别是什么?
JSP 共有以下6种基本动作 jsp:include：在页面被请求的时候引入一个文件。 jsp:useBean：寻找或者实例化一个JavaBean。   
jsp:setProperty：设置JavaBean的属性。 jsp:getProperty：输出某个JavaBean的属性。 jsp:forward：把请求转到一个新的页面。  
jsp:plugin：根据浏览器类型为Java插件生成OBJECT或EMBED标记。 

# 10.说一下表达式语言（EL）的隐式对象及其作用

EL的隐式对象包括：pageContext、initParam（访问上下文参数）、param（访问请求参数）、paramValues、header（访问请求头）、  
headerValues、cookie（访问cookie）、applicationScope（访问application作用域）、sessionScope（访问session作用域）、  
requestScope（访问request作用域）、pageScope（访问page作用域）。

用法如下所示：
  - ${pageContext.request.method}
  - ${pageContext["request"]["method"]}
  - ${pageContext.request["method"]}
  - ${pageContext["request"].method}
  - ${initParam.defaultEncoding}
  - ${header["accept-language"]}
  - ${headerValues["accept-language"][0]}
  - ${cookie.jsessionid.value}
  - ${sessionScope.loginUser.username}
   
# 11.请说明一下JSP中的静态包含和动态包含的有哪些区别？

1.两者格式不同，静态包含：<%@ include file="文件" %>，而动态包含：<jsp : include page = "文件" />。
2.包含时间不同，静态包含是先将几个文件合并，然后再被编译，缺点就是如果含有相同的标签，会出错。动态包含是页面被请求时编译，  
将结果放在一个页面。
3.生成的文件不同，静态包含会生成一个包含页面名字的servlet和class文件；而动态包含会各自生成对应的servlet和class文件
4.传递参数不同，动态包含能够传递参数，而静态包含不能 


# 12.过滤器有哪些作用和用法？
Java Web开发中的过滤器（filter）是从Servlet 2.3规范开始增加的功能，并在Servlet 2.4规范中得到增强。对Web应用来说，  
过滤器是一个驻留在服务器端的Web组件，它可以截取客户端和服务器之间的请求与响应信息，并对这些信息进行过滤。当Web容器  
接受到一个对资源的请求时，它将判断是否有过滤器与这个资源相关联。如果有，那么容器将把请求交给过滤器进行处理。在过滤器中，  
你可以改变请求的内容，或者重新设置请求的报头信息，然后再将请求发送给目标资源。当目标资源对请求作出响应时候，容器同样会  
将响应先转发给过滤器，在过滤器中你可以对响应的内容进行转换，然后再将响应发送到客户端。

常见的过滤器用途主要包括：对用户请求进行统一认证、对用户的访问请求进行记录和审核、对用户发送的数据进行过滤或替换、  
转换图象格式、对响应内容进行压缩以减少传输量、对请求或响应进行加解密处理、触发资源访问事件、对XML的输出应用XSLT等。


# 13.javaweb当中的监听器的作用？
就是对项目起到监听的作用，它能感知到包括request(请求域)，session(会话域)和applicaiton(应用程序)的初始化和属性的变化，  
活动在整个request和response周期中；其实监听器就是一种观察者模式。Servlet监听器用于监听一些重要事件的发生，监听器对象  
可以在事情发生前、发生后可以做一些必要的处理。下面将介绍几种常用的监听器

    分类及介绍：
    ServletContextListener：用于监听WEB 应用启动和销毁的事件，监听器类需要实现javax.servlet.ServletContextListener 接口。
    ServletContextAttributeListener：用于监听WEB应用属性改变的事件，包括：增加属性、删除属性、修改属性，监听器类需要实现
    javax.servlet.ServletContextAttributeListener接口。
    HttpSessionListener：用于监听Session对象的创建和销毁，监听器类需要实现javax.servlet.http.HttpSessionListener接口或者
    javax.servlet.http.HttpSessionActivationListener接口，或者两个都实现。
    HttpSessionActivationListener：用于监听Session对象的钝化/活化事件，监听器类需要实现javax.servlet.http.
    HttpSessionListener接口或者javax.servlet.http.HttpSessionActivationListener接口，或者两个都实现。（没用过）
    HttpSessionAttributeListener：用于监听Session对象属性的改变事件，监听器类需要实现
    javax.servlet.http.HttpSessionAttributeListener接口。 （没用过）
