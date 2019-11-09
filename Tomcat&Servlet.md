[TOC]

## 1.web相关概念回顾

1. 软件架构：
    1. C / S：客户端 / 服务器端
    2. B / S：浏览器 / 服务器端

2. 资源分类：
    1. 静态资源：所有用户访问后，得到的结果都是一样的，称为静态资源，静态资源可以直接被浏览器解析
        * 如：html、css、javascript
    2. 动态资源：每个用户访问相同资源后，得到的结果可能不一样，称为动态资源。动态资源被访问后，需要先转换为静态资源，再返回给浏览器
        * 如：servlet/jsp、php、asp……



3. 网络通信三要素：
    1. IP：电子设备（计算机）在网络中的唯一标识
    2. 端口：应用程序在计算机中的唯一标识。0~65535
    3. 传输协议：规定了数据传输的规则
        1. 基础协议：
            1. tcp：安全协议，三次握手。速度稍慢
            2. udp：不安全协议。速度块

## 2.web服务器软件

### 1.概述

* 服务器：安装了服务器软件的计算机
* 服务器软件：接受用户请求，处理请求，做出响应。
* web服务器软件：接受用户请求，处理请求，做出响应。
    * 在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目
    * 动态资源必须要在we服务器中运行，所以web服务器也叫 web容器

* 常见的Java相关的web服务器软件：
    * webLogic：Oracle公司，大型的Java EE服务器，支持所有的JavaEE规范，收费的。
    * webSphere：IBM公司，大型的Java EE服务器，支持所有的JavaEE规范，收费的。
    * JBOSS：JBOSS公司，大型的Java EE服务器，支持所有的JavaEE规范，收费的。
    * Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范 servlet / jsp。开源的，免费的。

* Java EE：Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范。


### 2.Tomcat：web服务器软件

1. 下载：https://tomcat.apache.org/
2. 安装：解压压缩包即可。
    * 注意：安装目录建议不要有中文和空格
    * tomcat目录：
    ![tomcat目录](https://ae01.alicdn.com/kf/H995df2614da84cd38a818715380d690ff.png)

3. 卸载：删除目录即可。
4. 启动：
    * bin/startup.bat，双击运行该文件即可
    * 访问：浏览器输入：http://127.0.0.1:8080 回车访问自己
    * http://别人的ip:8080：访问别人
    * 可能遇到的问题：
        1. 黑窗口一闪而过：
            * 原因：没有正确配置 JAVA_HOME变量
            * 解决方案：正确配置JAVA_HOME环境变量
        2. 启动报错：
            1. 暴力：找到占用的端口号，并且找到对应的进程，杀死该进程
                * `netstat -ano`
            2. 温柔：修改自身的端口号
                * conf / server.xml
                * `<Connector port="8080" protocol="HTTP/1.1"`
               `connectionTimeout="20000"`
                ` redirectPort="8443" />`
                * 一般会将tomcat的默认端口号修改为：80。80端口号是 http协议的默认端口号。
                    * 好处：在访问时，就不用输入端口号

5. 关闭：
    1. 正常关闭：
        * bin / shutdown.bat
        * Ctrl + c
    2. 强制关闭：
        * 点击窗口的 X

6. 配置
    * 部署项目的方式：
        1. 直接将项目放到 webapps 目录下即可。
            * /hello：项目的访问路径 --> 虚拟目录
            * 简化部署：将项目打成一个 war 包，再将 war 包放置到webapps目录下。
                * war 包会自动解压缩

        2. 配置 conf / server.xml文件
            * 在`<Host>`标签体中配置
            * `<Context docBase="D:\hello" path="/haha"/>`
            * docBase：项目存放的路径
            * path：虚拟路径

        3. 在 conf\Catalina\localhost 创建任意名称的 xml文件。在文件中编写
            * `<Context docBase="D:\hello"/>`
            * 虚拟目录：xml 文件的名称

* 静态项目和动态项目
    * 目录结构
        * java **动态项目**的目录结构：
            ```mysql
            -- 项目的根目录
                -- WEB-INF目录：
                    -- web.xml：web项目的核心配置文件
                    -- classes目录：放置字节码文件的目录
                    -- lib目录：放置依赖的 jar 包
            ```
* ***将 Tomcat 集成到 IDEA中，并且创建 JavaEE 的项目，部署项目***
  
    * **热部署**，就是在应用正在运行的时候升级软件，却不需要重新启动应用。

## 3.IDEA与tomcat的相关配置

1. **IDEA会为每一个tomcat部署的项目单独建立一份配置文件**
    * 查看控制台的log：Using CATALINA_BASE:   "C:\Users\用户名\.IntelliJIdea2019.1\system\tomcat\_itcast-JavaWeb"

2. **工作空间项目 和 tomcat 部署的web项目**
    * tomcat真正访问的是 “tomcat部署的web项目”，“tomcat部署的web项目” 对应着 “工作空间项目” 的web目录下的所有资源
    * 断点调试：使用“小虫子”启动 debug调试


## 4.Servlet

- **整个JavaWeb的核心知识体系都是围绕 Servlet 来转的**

### 1.概述

* 概念：运行在服务器端的小程序  Service Applet
    * Servlet就是一个接口，定义了 Java类被浏览器访问到(被tomcat识别)的规则
    * 将来我们定义一个类，实现 Servlet接口，复写方法，这个类就是Servlet

![Servlet概述](https://ae01.alicdn.com/kf/Hcac3fb07210b4d17a038c4ea6bd4176cc.png)

### 2.快速入门

1. **创建Java EE项目**
2. **定义一个类，实现Servlet接口**
   
    * `public class ServletDemo1 implements Servlet`
3. **实现接口中的抽象方法**
4. **配置Servlet，在web.xml中配置**：
   
    ```xml
        <!-- 配置Servlet -->
            <servlet>
                <servlet-name>demo1</servlet-name>
                <servlet-class>com.my.web.servlet.ServletDemo1</servlet-class>
        </servlet>
    
            <servlet-mapping>
                <servlet-name>demo1</servlet-name>
                <url-pattern>/demo1</url-pattern>
            </servlet-mapping>
    ```

### 3.Tomcat 执行 Servlet 的原理(过程)

1. 当服务器接受到客户端浏览器的请求后，会解析请求的URL路径，获取访问的Servlet的资源路径。
2. 查找 web.xml文件，是否有对应的 `<url-pattern>` 标签体内容
3. 如果有，根据`<servlet-name>`，再找到对应的 `<servlet-class>` 全类名
    * 如果在配置文件中出现 全类名 ，想到反射
4. tomcat会将字节码文件加载进内存(Class.forName("全类名"))，并且创建其对象
5. 调用其方法

* Servlet 执行原理：


![Servlet_执行原理](https://ae01.alicdn.com/kf/Hf954545712994441b158c0ce16ddd1c4O.png)

### 4.Servlet 中的生命周期

1. 被创建：执行 **init** 方法，执行一次
    1. **Servlet什么时候被创建？**   
        * **在默认情况下，第一次被访问时，Servlet被创建**
        * 可以配置指定执行 Servlet 的创建时机，在`<servlet>`标签下配置
             1. 在默认情况下，第一次被访问时，创建
                * `<load-on-startup>`的值为 负整数
            2. 在服务器启动时，创建
                * `<load-on-startup>`的值为 0 或 正整数
    
    2. **Servlet的 init 方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的**
        * **多个用户同时访问时，可能存在线程安全问题。**
        * **解决**：尽量不要在 Servlet中定义成员变量。即使定义了成员变量，也不要对其修改值。
    
2. 提供服务：执行 **service** 方法，执行多次
    * 每次访问Servlet时，service方法都会被调用一次。
3. 被销毁：执行 **destory** 方法，只执行一次
    * Servlet被销毁时执行。服务器关闭时，Servlet被销毁
    * 只有服务器**正常关闭**时，才会执行 destory 方法
    * destory 方法在 Servlet被销毁之前执行，一般用于释放资源

- web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">


    <!-- 配置Servlet -->
    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>com.my.web.servlet.ServletDemo1</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>

    <!-- 配置Servlet -->
    <servlet>
        <servlet-name>demo2</servlet-name>
        <servlet-class>com.my.web.servlet.ServletDemo2</servlet-class>
        <!--指定Servlet的创建时机
            1.在默认情况下，第一次被访问时，创建
                * <load-on-startup>的值为负整数
            2.在服务器启动时，创建
                * <load-on-startup>的值为 0 或 正整数
        -->
        <load-on-startup>6</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>demo2</servlet-name>
        <url-pattern>/demo2</url-pattern>
    </servlet-mapping>
</web-app>

```

- ServletDemo2.java

```java
package com.my.web.servlet;

import javax.servlet.*;
import java.io.IOException;

/**
 * Servlet的方法
 */

public class ServletDemo2 implements Servlet {

    /**
     * 初始化方法
     * 在 Servlet被创建时，执行。只会执行一次
     *
     * @param servletConfig
     * @throws ServletException
     */
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("init...");
    }

    /**
     * 获取 ServletConfig对象
     * ServletConfig：Servlet的配置对象
     *
     * @return
     */
    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /**
     * 提供服务的方法
     * 每一次Servlet被访问时，执行。执行多次
     *
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("service...");
    }

    /**
     * 获取Servlet的一些信息，版本，作者等等...
     *
     * @return
     */
    @Override
    public String getServletInfo() {
        return null;
    }

    /**
     * 销毁方法
     * 在服务器正常关闭时，执行。执行一次
     */
    @Override
    public void destroy() {
        System.out.println("destory...");
    }
}
```

### 5.Servlet 3.0

* 好处：
    * **支持注解配置。可以不需要 web.xml了。**
* 步骤：
    1. 创建JavaEE项目，选择 Servlet 的版本 3.0以上，可以不创建 web.xml
    2. 定义一个类，实现 Servlet 接口
    3. 覆盖重写 Servlet接口中的方法
    4. 在类上使用 @WebServlet 注解，进行配置
        * @WebServlet("Servlet 资源路径") 
        * 如：@WebServlet("/demo")
### 6.Servlet 体系结构

```
	Servlet			-- 接口
	  |		
	GenericServlet	-- 抽象类
	  |
	HttpServlet 	-- 抽象类
```

* **GenericServlet**：将 Servlet 接口中的其他方法做了默认空实现，只将 service()方法作为抽象方法
    * 将来定义 Servlet类时，可以继承 GenericServlet，实现 service()方法即可
* **HttpServlet**：对 http协议的一种封装，简化操作
    1. 定义类继承 HttpServlet
    2. 覆写 doGet / doPOst 方法

> **总结：**
>
> 实现 Servlet 接口，需要覆盖重写 其中的五个抽象方法
> GenericServlet 实现了 Servlet接口，对其中除 service方法外，做了空处理，我们只需要覆写 service方法即可。
> 但是 当用户提交数据时，我们需要判断提交的方式，是get，还是post，还是其他
> sun公司编写了一个抽象类 HttpServlet 继承自 GenericServlet，也是一个抽象类，但对 service方法做了处理，可以判断请求的方式是何种，不需要我们进行判断请求方式。
>
> ```java
> // 判断请求方式
> String method = req.getMethod();
> if ("GET".equals(method)) {
> 	// get方式获取数据
> 	doGet();
> } else if ("POST".equals(method)) {
> 	// post方式获取数据
> 	doPost();
> }
> ```
> 所以我们只需重写 **doGet()** 方法或者 **doPost()** 方法即可

**示例：**


* ServletDemo3.java

```java
@WebServlet("/demo3")
public class ServletDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doGet...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doPost...");
    }
}
```

* login.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>

<form action="/demo3" method="post">
    <input name="username">
    <input type="submit" value="提交">
</form>

</body>
</html>
```

### 7.Servlet 相关配置

1. urlpartten：Servlet访问路径
    1. 一个Servlet可以定义多个访问路径：
    2. 路径定义规则：
        1. /xxx
        2. /xxx/xxx：多层路径，目录结构
        3. *.后缀名
- 示例：

    ```java
    //@WebServlet("/demo4")
    @WebServlet({"/d4", "/dd4", "/ddd4"})
    //@WebServlet("/user/demo4")
    //@WebServlet("*.do") // * 前面不要加 /
    //@WebServlet("/demo4/*")
    public class ServletDemo4 extends HttpServlet {

        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            System.out.println("demo4...");
        }
    }
    ```

## 5.HTTP协议

### 1.概述

* 概念：Hyper Text Transfer Protocol 超文本传输协议
* **传输协议：定义了客户端和服务器端通信时，发送数据的格式**
* 特点：
    1. **基于 TCP/IP 的高级协议**
    2. **默认端口号：80**
    3. **基于请求/响应模型的**：一次请求对应一次响应，一一对应
    4. **无状态的**：每次请求之间相互独立，不能交互数据

* 历史版本：
    * 1.0：每一次请求都会建立新的连接
    * 1.1：复用连接

### 2.请求消息数据格式

#### 1.请求行
**请求方式 请求url 请求协议/版本**
**GET /login.html HTTP/1.1**

* ***请求方式***：
    * HTTP协议有7种请求方式，常用的有2种
        * **GET**：
            1. ***请求参数在请求行中，在 url 后***，GET请求方式没有请求体
            2. 请求的 url 的长度有限制的
            3. 不太安全
        * **POST**：
            1. ***请求参数在请求体中***
            2. 请求的 url 的长度没有限制的
            3. 相对安全
#### 2.请求头：**客户端浏览器告诉服务器一些消息**
**请求头名称：请求头值**（键值对形式）

* **常见的请求头：**
1. `User-Agent`：**浏览器告诉服务器，我访问你使用的浏览器版本信息**
2. `Referer`：http://localhost/login.html
    * **告诉服务器，我(当前请求头)从哪里来？**
        * **作用：**
            1. **防盗链**
        2. **统计工作**
    
#### 3.请求空行
**空行，就是用于分割POST请求的请求头，和请求体的**

#### 4.请求体(正文)
* **封装POST请求消息的请求参数的**

#### 请求字符串格式

```
POST /login.html HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:69.0) Gecko/20100101 Firefox/69.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: keep-alive
Referer: http://localhost/login.html
Upgrade-Insecure-Requests: 1

username=zhangsan
```

### 3.响应消息数据格式


#### 1.响应行
1. **组成：协议/版本 响应状态码 状态码描述**
2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态。
    1. **状态码都是3位数字**
    2. 分类：
        1. `1xx`：服务器接受客户端消息，但没有接受完成，等待一段时间后，发送1xx多状态码
        2. `2xx`：成功。代表：`200`
        3. `3xx`：重定向。代表：`302`(重定向)，`304`(访问缓存)
        4. `4xx`：客户端错误。
            * 代表：
                * `404`：请求路径没有对应的资源
                * `405`：请求方式没有对应的 **doXxx**方法
        5. `5xx`：服务器端错误。代表：`500`(服务器内部出现异常)


#### 2.响应头

1. 格式：头名称：值  
2. 常见的响应头：
    1. **Content-Type**：**服务器告诉客户端本次响应体数据格式以及编码格式**
    2. **Content-disposition**：**服务器告诉客户端以什么格式打开响应体数据**
        * 值：
            * **in-line**：**默认值**，在当前页面内打开
            * **attachment;filename=xxx**：以附件的形式打开响应体。文件下载


#### 3.响应空行

#### 4.响应体
传输的数据


#### 响应字符串格式

```
HTTP/1.1 200 
Set-Cookie: JSESSIONID=20E84EA998EFD0B9505A77527844E9D3; Path=/day15; HttpOnly
Content-Type: text/html;charset=UTF-8
Content-Length: 99
Date: Thu, 17 Oct 2019 07:48:04 GMT

<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  hello,response
  </body>
</html>
```


## 6.Request对象

### 1.**request 对象和 response 对象的原理**
1. **request 和 response 对象是由服务器创建的。我们来使用它们**
2. **request 对象是来获取请求消息，response 对象时来设置响应消息**

* 示例图：

![request&response原理](https://img13.360buyimg.com/uba/jfs/t1/47518/11/13464/599253/5da55065E62da7d0c/13311b286b45b1f3.png)


**过程：**
1. tomcat服务器会根据请求的 url中的资源路径，创建对应的 ServletDemo1 的对象
2. tomcat服务器，会创建request对象和response对象。request对象中封装请求消息数据。
3. tomcat将request和response两个对象传递个service方法，并且调用service方法。
4. 程序员，可以通过request对象获取请求消息数据，通过response对象设置响应消息数据。
5. 服务器在给浏览器作出响应之前，会从response对象中拿出程序员设置的响应消息数据。

### 2.request对象继承体系结构

```
ServletRequest	-- 接口
	| 继承
HttpServletRequest	-- 接口
	| 实现
org.apache.catalina.connector.RequestFacade 类（tomcat）
```

### 3.request功能

#### 1.获取请求消息数据
##### 1.获取请求行数据
* GET /day13/demo1?name=zhangsan HTTP/1.1
* 方法：
    1. 获取请求方式：GET
        * String getMethod()
    2. **(*)获取虚拟目录：/day13**
        * **String getContextPath()**
    3. 获取Servlet路径：/demo1
        * String getServletPath()
    4. 获取get方式请求参数：name=zhangsan
        * String getQueryString()
    5. **(*)获取请求URI：/day13/demo1**
        * **String getRequestURI()**： /day13/demo1
        * **String getRequestURL()**：http://localhost/day13/demo1
          * **URL：统一资源定位符**：http://localhost/day13/RequestDemo1		中华人民共和国
        * **URI：统一资源标识符**：/day13/RequestDemo1	共和国
    6. 获取协议及版本：HTTP/1.1
        * String getProtocol()
    7. 获取客户机的IP地址：
        * String getRemoteAddr()

##### 2.获取请求头数据
* 方法：
    * **(*)String getHeader(String name)：通过请求头的名称获取请求头的值**
    * Enumeration`<String>` getHeaderNames()：获取所有的请求头名称

* java.util 接口 Enumeration`<E>`
    * 方法
        * boolean hasMoreElements()：测试此枚举是否包含更多的元素。 
        * E nextElement()：如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素。 

> 注：此接口的功能与 Iterator 接口的功能是重复的。



##### 3.获取请求体数据

* **请求体：只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数**
* 步骤：
    1. 获取流对象
        * BufferedReader getReader()：获取字符输入流，只能操作字符数据
        * ServletInputStream getInputStream()：获取字节输入流，可以操作所有类型的数据
            * 在文件上传知识点后讲解
    2. 再从流对象中拿数据


#### 2.其他功能

##### 1.获取请求参数通用方式

1. **String getParameter(String name)**：**根据参数名称获取参数值**	username=zhangsan&password=123
2. String[] getParameterValues(String name)：根据参数名称获取参数值的数组	hobby=xx&hobby=game
3. Enumeration`<String>` getParameterNames()：获取所有请求的参数名称
4. **Map<String,String[]> getParameterMap()**：**获取所有参数的map集合**

* **中文乱码问题：**
    * get方式：tomcat 8 已经将get方式中文乱码问题解决了
    * post方式：会乱码
        * 解决：在获取参数前，设置 `request.setCharacterEncoding("utf-8");`

##### 2.请求转发

* **一种在服务器内部的资源跳转方式**

```java
request.getRequestDispatcher("Servlet资源路径").forward(request, response);
```

1. **步骤：**
    1. **通过 request 对象获取请求转发器对象：RequestDispatcher	getRequestDispatcher(String path)**
    2. **通过 RequestDispatcher 对象来进行转发：void forward(ServletRequest request, ServletResponse response)**

2. **特点：**
    1. **浏览器地址栏路径不发生变化**
    2. **只能转发到当前服务器内部资源中**
    3. **转发是一次请求，多个资源之间使用的是同一个请求**


##### 3.共享数据

* **域对象：一个有作用范围的对象，可以在范围内共享数据**
* **request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据**
* **方法：**
    1. **void setAttribute(String name,Object o)：存储数据**
    2. **Object getAttribute(String name)：通过键获取值**
    3. **void removeAttribute(String name)：通过键移除键值对**


##### 4.获取ServletContext
* **ServletContext getServletContext()**


## 7.案例:用户登录

### 1.用户登录案例需求

1. 编写login.html登录页面
	username & password 两个输入框
2. 使用Druid数据库连接池技术，操作mysql，day14数据库中user表
3. 使用JdbcTemplate技术封装JDBC
4. 登录成功跳转到SuccessServlet展示：登录成功！用户名，欢迎您
5. 登录失败跳转到FailServlet展示：登陆失败！用户名或密码错误

### 2.分析

![登录案例分析](https://img14.360buyimg.com/uba/jfs/t1/57106/21/13688/155449/5da7f16bEfa41be1c/ebedb75d0d1a4726.jpg)

### 3.开发步骤

#### 1.创建项目

创建JavaEE项目，导入 login.html 页面，druid.properties 配置文件，导入依赖 jar 包

* login.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>

<form action="/day14_test/loginServlet" method="post">
    用户名 <input type="text" name="username"><br/>
    密码 <input type="password" name="password"><br/>
    <input type="submit" value="登录">
</form>

</body>
</html>
```

* druid.properties
```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///db3
username=root
password=root
# 初始化连接数量
initialSize=5
# 最大连接数
maxActive=10
# 最大等待时间
maxWait=3000
maxIdle=8
minIdle=3
```

#### 2.创建数据库环境
```mysql
CREATE DATABASE day14;

USE day14;

CREATE TABLE USER (
	id INT PRIMARY KEY AUTO_INCREMENT, -- 编号
	username VARCHAR(32) NOT NULL UNIQUE, -- 用户名
	PASSWORD VARCHAR(32) NOT NULL -- 密码
);
DROP DATABASE IF EXISTS day14;

SELECT * FROM USER;
INSERT INTO USER(id,username,PASSWORD) VALUES(NULL, 'superbaby','123');
```

#### 3.创建包 com.my.domain，创建类User
```java
package com.my.domain;

/**
 * 用户的实体类
 */
public class User {
    private int id;
    private String username;
    private String password;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

#### 4.创建包 com.my.dao，创建类UsesrDao，提供login方法

```java
package com.my.dao;

import com.my.domain.User;
import com.my.util.JDBCUtils;
import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;

/**
 * 操作数据库中 User表的类
 */
public class UserDao {

    // 声明JDBCTemplate对象共用
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());

    /**
     * 登录方法
     *
     * @param loginUser 只有用户名和密码
     * @return user包含用户全部数据，没有查询到，返回 null
     */
    public User login(User loginUser) {
        User user = null;
        try {
            // 1.编写sql
            String sql = "select * from User where username = ? and password = ?";
            // 2.调用query方法
            user = template.queryForObject(sql,
                    new BeanPropertyRowMapper<User>(User.class),
                    loginUser.getUsername(), loginUser.getPassword());
            return user;
        } catch (DataAccessException e) {
            e.printStackTrace(); // 后期会记录在日志
            return null;
        }
    }
}
```

#### 5.编写 com.my.web.servlet.LoginServlet类

```java
package com.my.web.servlet;

import com.my.dao.UserDao;
import com.my.domain.User;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1.设置编码，防止中文乱码
        req.setCharacterEncoding("utf-8");
        // 2.获取请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        // 3.封装User对象
        User loginUser = new User();
        loginUser.setUsername(username);
        loginUser.setPassword(password);

        // 4.调用UserDao的login方法
        UserDao dao = new UserDao();
        User user = dao.login(loginUser);

        // 5.判断user
        if (null == user) {
            // 登录失败

            // 转发
            req.getRequestDispatcher("/failServlet").forward(req, resp);
        } else {
            // 登录成功

            // 存储数据
            req.setAttribute("user", user);
            // 转发
            req.getRequestDispatcher("/successServlet").forward(req, resp);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

#### 6.login.html中的form表单的action路径的写法

虚拟路径+Servlet资源路径

#### 7.BeanUtils工具类，简化数据封装
* 用于封装JavaBean的

1. **JavaBean：标准的Java类**
    1. 要求：
        1. 类必须被public修饰
        2. 必须提供空参的构造器
        3. 一对getter和setter方法
        4. 成员变量必须使用private修饰
    2. 概念：
        1. **成员变量**：
        2. **属性**：setter和getter方法截取后的产物
            * 例如：getUsername() --> Username --> username
2. 方法
    1. setProperties()：
    2. getProperties()：
    3. **populate(Object obj,Map map)**：将map集合的键值对信息，封装到对应的JavaBean对象中，把键当作属性名称，把值当作JavaBean对应的属性的值

## 8.Response对象

### 1.功能：设置响应消息

1. **设置响应行**
    1. **格式：HTTP/1.1 200 ok**
    2. **设置状态码：void setStatus(int sc)**
2. **设置响应头：void setHeader(String name, String value)**
3. **设置响应体：**
    * **使用步骤：**
        1. **获取输出流**
            * **字符输出流：PrintWriter getWriter()**
            * **字节输出流：ServletOutputStream getOutputStream()**
        2. **使用输出流，将数据输出到客户端浏览器**

### 2.案例

#### 1.完成重定向

* 重定向：资源跳转的方式

    ```java
    // 动态获取虚拟目录
    String contextPath = req.getContextPath();
    resp.sendRedirect(contextPath+"/Servlet资源路径");
    ```

* 代码实现:

    ```java
    /*
            // 1.设置状态码为302 重定向
            resp.setStatus(302);
            // 2.设置响应头location
            resp.setHeader("location", "/day15/responseDemo2");
    */

            // 简单的重定向方法
            resp.sendRedirect("/day15/responseDemo2");
    ```
    
* 重定向的特点：**redirect**
    1. 地址栏路径发生变化
    2. 重定向可以访问其他站点(服务器)的资源
    3. **重定向是两次请求，不能使用request对象来共享数据**

* 转发的特点：**forward**
    1. 地址栏路径不变
    2. 转发只能访问当前服务器下的资源
    3. 转发是一次请求，可以使用一个request对象来共享数据

* forward 和 redirect 的区别(将来面试题会这样问?)

* 请求转发

![请求转发](https://img14.360buyimg.com/uba/jfs/t1/83342/39/13369/47926/5da94534Ea9643e88/ec9de23dbaa87b99.png)


* 重定向

![重定向](https://img14.360buyimg.com/uba/jfs/t1/99933/24/210/56319/5da94534Ef2851b3d/7a300e1055531c06.png)

* 路径写法(重要)：
    1. 路径分类
        1. 相对路径：通过相对路径不可以确定唯一资源
            * 如：./index.html
            
            * 不以 / 开头，以 . 开头路径
            
                ```html
    <a href="./responseDemo2"> <!-- 不写，默认是以./开头 -->
                <a href="../responseDemo2">
                ```
            
            * 规则：找到当前资源和目标资源之间相对位置关系
                * `./`：当前目录
                * `../`：后退一级目录
            
        2. 绝对路径：通过绝对路径可以确定唯一资源
            * 如：http://localhost/day15/responseDemo2	/day15/responseDemo2
            * 以 / 开头的路径
            * 规则：判断定义的路径是给谁用的？判断请求将来从哪发出
                * 给客户端浏览器使用：需要加虚拟目录(项目的访问路径)
                    * **建议虚拟目录动态获取：request.getContextPath();**
                    * `<a>`，`<form>`，重定向...
                * 给服务器使用：不需要加虚拟目录
                    * 转发路径

> 后期写 jsp页面时，推荐使用绝对路径写法，因为相对路径写起来比较麻烦



#### 2.服务器输出字符数据到浏览器

* 步骤：
    1. 获取字符输出流
    2. 输出数据

* 注意：
    * 乱码问题：
      
        * 乱码原因：编码和解码使用的字符集不同
        
    1. PrintWriter pw = response.getWriter()：获取的流的默认编码是 ISO-8859-1
        2. 设置该流的默认编码
        3. 告诉浏览器响应体使用的编码
    
    ```java
    // 简单的形式，设置编码，是在获取流之前设置
    response.setContentType("text/html;charset=utf-8");
    ```

* 示例
    ```java
    // 获取流对象PrintWriter之前，设置流的默认编码：ISO-8859-1 设置为：GBK
    // resp.setCharacterEncoding("utf-8");

    // 告诉浏览器，服务器发送消息体的数据的编码，建议浏览器使用该编码解码
    // resp.setHeader("content-type", "text/html;charset=utf-8"); 
/*
    resp.setHeader() 等效于 resp.setCharacterEncodin() & resp.setHeader()两行
    */

    // 简单的形式，设置编码，是在获取流之前设置
    resp.setContentType("text/html;charset=utf-8");// 一行顶两行
    // 1.获取字节输出流
    PrintWriter pw = resp.getWriter();
    // 2.输出数据
    pw.write("<h1>hello response</h1>");
    pw.write("你好啊 response");
    ```



#### 3.服务器输出字节数据到浏览器

* 步骤：
    1. 获取字节输出流
    2. 输出数据

* 示例

```java
        resp.setContentType("text/html;charset=utf-8");
        // 1.获取字节输出流
        ServletOutputStream sos = resp.getOutputStream();
        // 2.输出数据
        sos.write("你好".getBytes("utf-8")); // getBytes()默认使用的是gbk
```

#### 4.验证码

```java
/**
 * 生成验证码
 */
@WebServlet("/checkCodeServlet")
public class CheckCodeServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.创建一对象，代表内存中的图片(验证码图片对象)
        int width = 100;
        int height = 50;
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);

        // 2.美化图片
        // 2.1 填充背景色
        Graphics g = image.getGraphics(); // 画笔对象
        g.setColor(Color.pink); // 设置画笔颜色
        g.fillRect(0, 0, width, height);

        // 2.2 画边框
        g.setColor(Color.BLUE);
        g.drawRect(0, 0, width-1, height-1);

        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        Random ran = new Random();
        for (int i = 1; i <= 4; i++) {
            // 生成随机角标
            int index = ran.nextInt(str.length());
            char ch = str.charAt(index); // 随机字符
            // 2.3 写验证码
            g.drawString(ch+"", width/5*i, height/2);
        }

        // 2.4 画干扰线
        g.setColor(Color.green);

        // 随机生成坐标点
        for (int i = 0; i < 10; i++) {
            int x1 = ran.nextInt(width);
            int x2 = ran.nextInt(width);

            int y1 = ran.nextInt(height);
            int y2 = ran.nextInt(height);
            g.drawLine(x1, y1, x2, y2);
        }

        // 3.将图片输出到页面展示
        ImageIO.write(image, "jpg", response.getOutputStream());

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

## 9.ServletContext对象

1. 概念：这个对象代表整个web应用，可以和程序的容器(服务器)来通信
2. 获取
    * 1.通过request对象获取
        * request.getServletContext();
    * 2.通过HttpServlet获取
        * this.getServletContext();

3. 功能：
    1. **获取MIME类型**：
       
        * **MIME类型：在互联网通信过程中定义的一种文件数据类型**
            * 格式：**大类型/小类型**	text/html	image/jpeg
* 获取：String getMimeType(String file)
        
    2. **域对象：共享数据**
        1. setAttribute(String name, Object value)
        2. getAttribute(String name)
        3. removeAttribute(String name)
    * **ServletContext对象范围：所有用户所有请求的数据**
    
    3. **获取文件的真实(服务器)路径**

```java
// 通过HttpServlet获取ServletContext对象
ServletContext context = this.getServletContext();

// 获取文件的服务器路径
String realPath = context.getRealPath("/b.txt"); // web目录下的资源访问
System.out.println(realPath);

String c = context.getRealPath("/WEB-INF/c.txt"); // WEB-INF目录下的资源访问
 System.out.println(c);

/*
     将来src目录下的资源都会放到web/WEB-INF/classes目录下
     也可以使用类加载器获取src目录下的资源，但类加载器只能获取src目录下，不能获取到web目录下，受限
*/
String a = context.getRealPath("/WEB-INF/classes/a.txt");// src目录下的资源访问
System.out.println(a);
```



## 10.案例

1. **文件下载需求**：
	1. 页面显示超链接
	2. 点击超链接后弹出下载提示框
	3. 完成图片文件下载
	
* **分析**：
	1. 超链接指向的资源如果能被浏览器解析，则在浏览器中展示，如果不能解析，则弹出下载提示框。不满足需求
	2. 任何资源的下载都必须弹出下载提示框
	3. 使用响应头设置资源的打开方式：
		* content-disposition:attachment;filename=xxx
	
* **步骤**：
	1. 定义页面，编辑超链接href属性，指向Servlet，传递资源名称filename
	2. 定义Servlet
		1. 获取文件名称(获取request参数)
		2. 使用字节输入流加载文件进内存
		3. 指定response的响应头：content-disposition:attachment;filename=xxx
		4. 将数据写出到response输出流

* 问题：
    * 中文文件问题
        * 解决思路：
            1. 获取客户端使用的浏览器版本信息
            2. 根据不同的版本信息，设置filename的编码方式不同

**代码实现**

* download.html

```html
<a href="/day15/img/1.jpg">图片</a>
<br/>
<a href="/day15/img/2.flv">视频</a>
<hr/>
<!-- get请求在tomcat8中已解决中文乱码问题 -->
<a href="/day15/downloadServlet?filename=1.jpg">图片1</a>
<br/>
<a href="/day15/downloadServlet?filename=九尾.jpg">九尾</a>
<br/>
<a href="/day15/downloadServlet?filename=2.flv">视频</a>
```

* DownloadServlet.java

```java
package com.my.web.download;

import com.my.web.utils.DownLoadUtils;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileInputStream;
import java.io.IOException;

/**
 * 附件下载案例
 */
@WebServlet("/downloadServlet")
public class DownloadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1.获取请求参数，文件名称
        String filename = request.getParameter("filename");
        // 2.使用字节输入流加载文件进内存
        // 2.1 获取ServletContext对象
        ServletContext servletContext = this.getServletContext();
        // 2.2 获取文件服务器真实路径
        String realPath = servletContext.getRealPath("/img/" + filename);

        // 2.3 使用字节流关联
        FileInputStream fis = new FileInputStream(realPath);

        // 3. 设置response的响应头
        String mimeType = servletContext.getMimeType(filename); // 获取文件扩展名对应的mime类型

        // 3.1 设置响应头的类型：content-type
        response.setHeader("content-type", mimeType);

        // 3.2 设置响应头的打开方式：content-disposition

        // 解决中文文件名问题
        // 1.获取user-agent请求头
        String agent = request.getHeader("user-agent");
        // 2.使用工具类方法编码文件名即可
        filename = DownLoadUtils.getFileName(agent, filename);

        response.setHeader("content-disposition", "attachment;filename=" + filename);

/*
        // 简单的形式，设置编码，是在获取流之前设置
        response.setContentType("text/html;charset=utf-8");
*/
        // 4. 将输入流的数据写出到输出流中
        ServletOutputStream sos = response.getOutputStream();
        byte[] buff = new byte[1024 * 6]; // 缓冲数组
        int len = 0; // 读取的有效个数
        while ((len = fis.read(buff)) != -1) {
            sos.write(buff, 0, len);
        }
        fis.close();

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```
