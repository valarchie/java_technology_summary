- 该部分内容已比较老旧

## 启动项目时如何实现不在链接里输入项目名就能启动?
修改Tomcat配置文件 server.xml。将Context标签内的path属性改为“/”即可。

## 一分钟之内只能处理1000个请求，你怎么实现，手撕代码?
每处理完一个请求，就往Application作用域内的一个LinkedBlockingQueue中添加一个时间戳。
每接收到一个请求的时候，就先判断Application作用域当中的LinkedBlockingQueue当中的size是否大于1000，判断前先remove  
掉60秒之前的时间戳。


## 什么时候用assert？
在开发和测试过程中，很多时候我们需要对某些场景进行判断，当这个判断满足我们预期的答案，才继续往下执行，否则终止程序或者发出警告。  
比较常用在单元测试。

## Java应用服务器有哪些？
WebLogic， WebSphere， Jboss， Tomcat

## JSP内置对象及常用方法？

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

## JSP的四个域对象的作用范围？
application、session、request、page

## JSP 和 Servlet 有哪些相同点和不同点，他们之间的联系是什么？
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
    
## ★四种会话跟踪技术

1. 隐藏表单域：<input type="hidden">，非常适合步需要大量数据存储的会话应用。
2. URL重写: URL可以在后面附加参数抛回给客户端，客户端在下一次请求携带回给服务器。
3. Cookie:一个 Cookie 是一个小的，已命名数据元素。服务器使用 SET-Cookie 头标将它作为 HTTP
响应的一部分传送到客户端，客户端被请求保存 Cookie 值，在对同一服务器的后续请求使用一个
Cookie 头标将之返回到服务器。与其它技术比较，Cookie 的一个优点是在浏览器会话结束后，甚至
在客户端计算机重启后它仍可以保留其值。
4. Session：使用 setAttribute(String str,Object obj)方法将对象捆绑到一个会话

## 说说weblogic中一个Domain的缺省目录结构?比如要将一个简单的helloWorld.jsp放入何目录下,然后在浏览器上就可打入主机？
Domain目录\服务器目录\applications，将应用目录放在此目录下将可以作为应用访问，如果是web应用，应用目录需要满足Web应用  
目录要求，jsp文件可以直接放在应用目录中，JavaBean需要放在应用目录的WEB-INF目录的classes目录中，设置服务器的缺省应用将  
可以实现在浏览器上无需输入应用名。

## jsp有哪些动作?作用分别是什么?
JSP 共有以下6种基本动作 
- jsp:include：在页面被请求的时候引入一个文件。 
- jsp:useBean：寻找或者实例化一个JavaBean。   
- jsp:setProperty：设置JavaBean的属性。 
- jsp:getProperty：输出某个JavaBean的属性。 
- jsp:forward：把请求转到一个新的页面。  
- jsp:plugin：根据浏览器类型为Java插件生成OBJECT或EMBED标记。 

## 说一下表达式语言（EL）的隐式对象及其作用

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
   
## 请说明一下JSP中的静态包含和动态包含的有哪些区别？

1.两者格式不同，静态包含：<%@ include file="文件" %>，而动态包含：<jsp : include page = "文件" />。
2.包含时间不同，静态包含是先将几个文件合并，然后再被编译，缺点就是如果含有相同的标签，会出错。动态包含是页面被请求时编译，  
将结果放在一个页面。
3.生成的文件不同，静态包含会生成一个包含页面名字的servlet和class文件；而动态包含会各自生成对应的servlet和class文件
4.传递参数不同，动态包含能够传递参数，而静态包含不能 


## ★过滤器有哪些作用和用法？
Java Web开发中的过滤器（filter）是从Servlet 2.3规范开始增加的功能，并在Servlet 2.4规范中得到增强。对Web应用来说，  
过滤器是一个驻留在服务器端的Web组件，它可以截取客户端和服务器之间的请求与响应信息，并对这些信息进行过滤。当Web容器  
接受到一个对资源的请求时，它将判断是否有过滤器与这个资源相关联。如果有，那么容器将把请求交给过滤器进行处理。在过滤器中，  
你可以改变请求的内容，或者重新设置请求的报头信息，然后再将请求发送给目标资源。当目标资源对请求作出响应时候，容器同样会  
将响应先转发给过滤器，在过滤器中你可以对响应的内容进行转换，然后再将响应发送到客户端。

常见的过滤器用途主要包括：对用户请求进行统一认证、对用户的访问请求进行记录和审核、对用户发送的数据进行过滤或替换、  
转换图象格式、对响应内容进行压缩以减少传输量、对请求或响应进行加解密处理、触发资源访问事件、对XML的输出应用XSLT等。

## ★拦截器与过滤器的区别？
拦截器基于反射，过滤器基于函数回调。
拦截器依赖于controller，过滤器依赖于servlet。
总而言之就是请求进来  fileter-->servlet-->inteceptor-->controller


## javaweb当中的监听器的作用？
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
    
## web.xml文件中可以配置哪些内容？
web.xml用于配置Web应用的相关信息，如：监听器（listener）、过滤器（filter）、 Servlet、相关参数、会话超时时间、  
安全验证方式、错误页面等。


## ★forward与redirect区别，说一下你知道的状态码，redirect的状态码是多少？
forward是服务器内部转发，redirect是服务器让客户端转发。redirect的状态码是301/302.
- 200：表示请求已成功，请求所希望的响应头或数据体将随此响应返回
- 202：服务器已接受请求，但尚未处理
- 301：被请求的资源已永久移动到新位置
- 302：请求的资源临时从不同的URI响应请求，但请求者应继续使用原有位置来进行以后的请求。一般重定向都是这个状态码。
- 304：自从上次请求后，请求的网页未修改过。如jq文件，logo
- 403：服务器已经理解请求，但是拒绝执行它
- 404：请求失败，请求所希望得到的资源未被在服务器上发现
- 500：服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般是服务器的程序码出错时出现
- 502： 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。例如Nginx转发请求后，无法  
收到或理解响应时，返回的状态码。
- 503：由于临时的服务器维护或者过载，服务器暂时无法处理请求
- 504： 作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器收到响应。例如当Nginx超过配置的超时时间  
还没有收到响应时，就返回504错误。


## ★转发和重定向有什么区别？
转发是服务端的行为，重定向是客户端的行为。

## servlet生命周期，是否单例，为什么是单例。
生命周期：加载并实例化、初始化、请求处理、销毁。  是单例。为了有效利用JVM允许多个线程访问同一个实例的特性，  
来提高服务器性能。在非分布式系统中，Servlet容器只会维护一个Servlet的实例。


## 什么是Servlet?
servlet是运行于tomcat容器或其他servlet容器中的WEB组件，用于处理请求和响应请求。所以我们就不需要关心  
如何接受请求和响应请求，只需要关心于我们的业务逻辑。

## 说出Servlet的生命周期，并说出Servlet和CGI的区别。
Servlet 生命周期  
1、加载  2、实例化  3、初始化  4、处理请求  5、销毁
区别：  
Servlet处于服务器进程中，它通过多线程方式运行其service方法，一个实例可以服务于多个请求，并且其实例一般不会销毁，  
而CGI对每个请求都产生新的进程，服务完成后就销毁，所以效率上低于servlet。  


## Servlet执行时一般实现哪几个方法？
- public void init(ServletConfig config)  
- public ServletConfig getServletConfig()  
- public String getServletInfo()
- public void service(ServletRequest request,ServletResponse response)
- public void destroy() 


## Servlet 3中的异步处理指的是什么？
不阻塞用户请求，请求立即得到响应，而处理结果在完成之后再返回给用户。

## 服务器收到用户提交的表单数据，到底是调用Servlet的doGet()还是doPost()方法？
根据用户请求的方法是Get还是POST。

## MVC 的各个部分都有那些技术来实现?如何实现?
MVC 是 Model－View－Controller 的简写。 Model 代表的是应用的业务逻辑（ 通过JavaBean，  
EJB 组件实现）， View 是应用的表示面（ 由 JSP 页面产生）， Controller 是提供应用的处理  
过程控制（ 一般是一个 Servlet）， 通过这种设计模型把应用逻辑， 处理过程和显示逻辑分成不同  
的组件实现。 这些组件可以进行交互和重用。 


## 什么是DAO模式？

DAO 模式：
DAO (DataAccessobjects 数据存取对象)是指位于业务逻辑和持久化数据之间实现对持久化数据的访问。  
通俗来讲，就是将数据库操作都封装起来。  

对外提供相应的接口  
在面向对象设计过程中，有一些"套路”用于解决特定问题称为模式。  

DAO 模式提供了访问关系型数据库系统所需操作的接口，将数据访问和业务逻辑分离对上层提供面向对象的数据访问接口。  

从以上 DAO 模式使用可以看出，DAO 模式的优势就在于它实现了两次隔离。 


    1、隔离了数据访问代码和业务逻辑代码。业务逻辑代码直接调用DAO方法即可，完全感觉不到数据库表的存在。
    分工明确，数据访问层代码变化不影响业务逻辑代码,这符合单一职能原则，降低了耦合性，提高了可复用性。
    2、隔离了不同数据库实现。采用面向接口编程，如果底层数据库变化，如由 MySQL 变成 Oracle 只要增加
    DAO 接口的新实现类即可，原有 MySQ 实现不用修改。这符合 "开-闭" 原则。该原则降低了代码的藕合性，
    提高了代码扩展性和系统的可移植性。

一个典型的DAO 模式主要由以下几部分组成。 

    1、DAO接口： 把对数据库的所有操作定义成抽象方法，可以提供多种实现。
    2、DAO 实现类： 针对不同数据库给出DAO接口定义方法的具体实现。
    3、实体类：用于存放与传输对象数据。
    4、数据库连接和关闭工具类： 避免了数据库连接和关闭代码的重复使用，方便修改。 
    
    
## get和post区别？
    在的HTTP规范中，Post用于上传数据、Get用于请求数据。Post将数据封装在body，Get讲数据直接放在url后面进行传输，
    所以这一点上面get稍微比较不安全，但两者皆是明文传输。Get一般有数据长度的限制，但这个限制并不是Http制定的，
    而是各大浏览器厂商制定的，而post的请求长度有服务端来配置。一般而言post可传输的数据比较大。
    
## ★cookies和session的区别？
    简单的说，由于HTTP是无状态的，所以设计出两种会话方案。Cookie是客户端的会话技术，Session是服务端的会话技术。
    一个储存在浏览器，一个存储在服务器内存中。Cookie非安全，可窃取到，session安全，不可窃取到。Cookies有大小数
    量的限制，Session限制于服务器的内存。
    
## 如何设置请求的编码以及响应内容的类型？
    通过请求对象（ServletRequest）的setCharacterEncoding(String)方法可以设置请求的编码，其实要彻底解决乱码
    问题就应该让页面、服务器、请求和响应、Java程序都使用统一的编码，最好的选择当然是UTF-8；通过响应对象
    （ServletResponse）的setContentType(String)方法可以设置响应内容的类型，当然也可以通过HttpServletResponse
    对象的setHeader(String, String)方法来设置。

## GBK和UTF8编码的区别？
1. GBK包含全部中文字符；UTF-8则包含全世界所有国家需要用到的字符。
2. GBK是在国家标准GB2312基础上扩容后兼容GB2312的标准（好像还不是国家标准）；  
而UTF-8编码的文字可以在各国各种支持UTF8字符集的浏览器上显示。

## 什么是WebService?
    WebService是一种跨编程语言和跨操作系统平台的远程调用技术。
    WebService就是一个应用程序向外界暴露出一个能通过Web进行调用的API，也就是说能用编程的方法通过Web来调用这个
    应用程序。我们把调用这个WebService的应用程序叫做客户端，而把提供这个WebService的应用程序叫做服务端。
    WebService 即是HTTP + XML 的公共接口。
    因为在很久之前，web上的网页都是之间生成整个网页抛回给浏览器。所以在当时规范了WebService这一协议，
    在访问的时候，只返回没有经过渲染的核心数据。
    发布WebService，可使用JWS包进行发布。xml的格式进行交换数据。

