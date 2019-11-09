[TOC]

## 1. 会话技术

1. 会话：一次会话包含多次请求和响应。
    * 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止
2. **功能：在一次会话的范围内的多次请求间，共享数据**
3. 方式：
    1. 客户端会话技术：Cookie 
    2. 服务器端会话技术：Session


### 1. Cookie

#### 1. 快速入门

1. 概念：客户端会话技术，将数据保存到客户端
2. 快速入门：
    * 使用步骤：
        1. 创建Cookie对象，绑定数据
            * new Cookie(String name, String value)
        2. 发送Cookie对象
            * void addCookie(Cookie cookie)
            * 例如： response.addCookie(Cookie cookie)
        3. 获取Cookie，拿到数据
            * Cookie[ ] getCookies()
            * 例如：Cookie[ ] request.getCookies();

#### 2. 实现原理

Cookie原理分析：
基于响应头：set-cookie 和 请求头：cookie

![Cookie_原理分析](https://img11.360buyimg.com/uba/jfs/t1/106356/20/628/221204/5db046fcEa7508f97/f7b6c3b85c6e713a.png)


#### 3. cookie的细节

##### 1.一次可不可以发送多个cookie？
* 可以
* 可以创建多个 cookie 对象，使用 response 对象多次调用 addCookie 方法发送 cookie 即可。

* 示例:

```java
@WebServlet("/cookieDemo3")
public class CookieDemo3 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建cookie对象
        Cookie c1 = new Cookie("msg", "java");
        Cookie c2 = new Cookie("name", "zhangsan");

        // 通过response对象，发送cookie对象
        response.addCookie(c1);
        response.addCookie(c2);

    }
```

```java
@WebServlet("/cookieDemo2")
public class CookieDemo2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 3.获取Cookies，遍历获取
        Cookie[] cs = request.getCookies();
        if (cs != null) { // 判断为非空
            for (Cookie c : cs) {
                String name = c.getName();
                String value = c.getValue();
                System.out.println(name + ":" + value);
            }
        }
    }
```


##### 2.cookie在浏览器中保存多长时间？

1. 默认情况下，当浏览器关闭后，Cookie数据被销毁
2. 持久化存储
    * void setMaxAge(int seconds)
        1. 正数：将 Cookie 数据写到硬盘的文件中。持久化存储。 seconds：cookie的存活时间，单位为：秒
        2. 负数：默认值
        3. 零：删除 Cookie 信息

* 示例：

```java
// 创建cookie对象，存储数据
Cookie c = new Cookie("name", "zhangsan");

// 设置cookie生命周期（存活时间）
// c.setMaxAge(40); // 持久化存储到硬盘，40秒后会自动删除cookie文件
// c.setMaxAge(-1); // 默认浏览器关闭，cookie文件被删除
c.setMaxAge(0); // 删除cookie

// 发送cookie对象
        response.addCookie(c);
```


##### 3.cookie能不能存中文？

* 在 tomcat 8 之前，cookie中不能直接存储中文数据。
    * 需要将中文数据转码 ---- 一般采用URL编码（%E3）
* 在 tomcat 8 之后，cookie支持中文数据

##### 4.cookie共享问题？

1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie能不能共享？
    * 默认情况下cookie不能共享
    * void setPath(String path)：设置cookie的获取范围。默认情况下，设置当前的虚拟目录
        * 如果要共享，则可以将path设置为："/"
2. 不同的tomcat服务器间cookie共享问题？
    * void setDomain(String path)：如果设置一级域名相同，那么多个服务器之间cookie可以共享
        * 例如：setDomain(".baidu.com")，那么tieba.baidu.com 和 news.baidu.com中cookie可以共享。

* 示例：

```java
Cookie c = new Cookie("msg" ,"你好");
// c.setPath("/day16"); // 默认是在当前虚拟目录
c.setPath("/"); // 在同一个服务器不同web项目下共享数据
response.addCookie(c);
```

#### 4. Cookie的特点和作用

1. cookie存储数据在客户端浏览器
2. 浏览器对于单个 cookie 的大小有限制（4KB），以及对同一域名下的总的 cookie 数量也有限制（对于浏览器的不同，限制不同，一般20个）

* 作用：
    1. cookie 一般用于存储少量的不太敏感的数据
    2. 在不登录的情况下，完成服务器对客户端的身份识别

如果登录了，数据会保存在数据库中

#### 5. 案例:记住上一次的访问时间

1. 需求：
    1. 访问一个Servlet，如果是第一次访问，则提示：您好，欢迎您首次访问。
    2. 如果不是第一次访问，则提示：欢迎回来，您上次访问时间为：显示时间字符串

2. 分析：
    1. 可以采用 Cookie 来完成
    2. 在服务器中的Servlet判断是否有一个名为 lastTime 的cookie
        1. 有：不是第一次访问
            1. 响应数据：您好，欢迎您首次访问。
            2. 写回Cookie：lastTime=10点25分
        2. 没有：是第一次访问
            1. 响应数据：欢迎回来，您上次访问时间为：10点28分
            2. 写回Cookie：lastTime=10点28分

3. 代码实现：

```java
package com.my.cookie;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.text.SimpleDateFormat;
import java.util.Date;

@WebServlet("/cookieTest")
public class CookieTest extends HttpServlet {

    /**
     * 更新时间
     *
     * @return 格式化的时间字符串格式
     * @throws UnsupportedEncodingException
     */
    private String updateTime() throws UnsupportedEncodingException {
        // 写回cookie，更新时间
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        String str_date = sdf.format(new Date());
        // URL编码
        str_date = URLEncoder.encode(str_date, "utf-8");
        return str_date;
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置响应的消息体的数据格式以及编码
        response.setContentType("text/html;charset=utf-8");

        // 1.获取所有的cookie
        Cookie[] cookies = request.getCookies();
        // 2.遍历cookies
        boolean flag = false; // 没有cookie，则为false
        if (cookies != null && cookies.length > 0) {
            for (Cookie cookie : cookies) {
                // 3.获取cookie的名称
                String name = cookie.getName();
                // 4.判断名称是否是：lastTime
                if ("lastTime".equals(name)) {
                    // 有该cookie，不是第一次访问
                    flag = true;
                    System.out.println("有cookie");
                    // 响应数据
                    String value = cookie.getValue();
                    // URL解码
                    value = URLDecoder.decode(value, "utf-8");
                    response.getWriter().write("<h1>您上次的访问时间为：" + value + "</h1>");
//                    String value1 = cookie.getValue();

//                    System.out.println(value1.getBytes().length);

//                    System.out.println(cookie.getValue().getBytes().length);
                    String str_date = this.updateTime();
                    cookie.setValue(str_date);
                    // 设置cookie的存活时间
                    //cookie.setMaxAge(0); // 删除cookie
                    response.addCookie(cookie); // 发送cookie
                    // 找到lastTime的cookie后，退出，不再继续循环判断
                    break;
                }
            }
            if (cookies == null || cookies.length == 0 || flag == false) {
                // 没有lastTime的cookie，第一次访问

                System.out.println("没有cookie，设置cookie");
                // 响应数据
                response.getWriter().write("<h1>您好，欢迎首次登录！</h1>");

                String str_date = this.updateTime();
                Cookie cookie = new Cookie("lastTime", str_date);
//                System.out.println(cookie.getValue().length());
                // 设置cookie的存活时间
                cookie.setMaxAge(60 * 60 * 24 * 30); // cookie存活1个月
                response.addCookie(cookie); // 发送cookie
            }
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 2. Session

1. 概念：服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。HttpSession

#### 1. 快速入门

1. 获取 HttpSession 对象：
    * HttpSession session = request.getSession();
2. 使用 HttpSession 对象：
    * Object getAttribute(String name)
    * void setAttribute(String name, Object value)
    * void removeAttribute(String name)

#### 2. Session原理

**Session的实现是依赖于Cookie的**

> **服务器是如何确保在一次会话范围内，多次获取的Session对象是同一个？**
>
> 在第一次获取Session时，没有Cookie，服务器会在内存中创建一个新的Session对象
> Session对象有一个JSESSIONID，借助于 set-cookie 这个头，将JSESSIONID响应给客户端浏览器，当客户端浏览器第二次访问时，会 request 头中自动带着 cookie 头，cookie头中保存的JSESSIONID会被服务器自动检索，在服务器中查找是否有相同值的JSESSIONID的Session，如果有就返回这个 JSESSIONID对象的引用
> request.getSession();，该方法返回一个HTTPSession，也就是 JSESSIONID


![Session_原理分析](https://img13.360buyimg.com/uba/jfs/t1/73293/17/13747/151809/5db19a65E4ba682cc/834b0edcff6b7abb.jpg)

#### 3. Session细节

##### 1. 当客户端关闭后，服务器不关闭，两次获取session是否为同一个？

默认情况下，不是。
如果需要相同，则可以创建 Cookie，键为 JSESSIONID，值为 session.getId()，设置最大存活时间，让 Cookie 持久化存储

```java
// 1.获取session
HttpSession session = request.getSession();
// 确保浏览器关闭后，再次访问获取的是同一个session，依赖于cookie的响应头和请求头
Cookie cookie = new Cookie("JSESSIONID", session.getId());
// 持久化存储1小时
cookie.setMaxAge(60 * 60);
System.out.println(session);
```

##### 2. 客户端不关闭，服务器关闭后，两次获取的session是同一个吗？

不是同一个，但是要确保数据不丢失

* session的钝化（对象的序列化）：
    * 在服务器正常关闭之前，将session对象序列化到硬盘上。
* session的活化（对象的反序列化）：
    * 在服务器启动后，将session文件转化为内存中的session对象即可。

**小提示：**

1. session的钝化和活化在本地tomcat 8 中已经自动实现了，会在work（存放服务为动态生成的数据，例如：jsp，session的序列化SESSIONS）中
2. session的钝化在IDEA中可以，但活化不行，原因是IDEA一旦重启，会自动删除work目录，然后重行创建一个空的work目录，将原本序列化好的SESSIONS删除


##### 3. session什么时候被销毁？

1. 服务器关闭
2. session对象调用 invalidate()
3. session默认失效时间：30分钟

* 选择性配置修改
```xml
  <!-- ==================== Default Session Configuration ================= -->
  <!-- You can set the default session timeout (in minutes) for all newly   -->
  <!-- created sessions by modifying the value below.                       -->
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```

#### 4. Session特点

1. session用于存储一次会话的多次请求间的数据，存在服务器端
2. session可以存储任意类型（Object），任意大小的数据

* session与cookie的区别：
    1. session存储数据在服务器端，cookie在客户端
    2. session没有数据大小限制，cookie有
    3. session没有数据安全，cookie相对于不安全






## 2. JSP

### 1. 入门学习

1. 概念：
    * Java Server Pages：java服务器端页面
    * 可以理解为：一个特殊的页面，其中既可以指定定义 html 标签，又可以定义 java 代码
    * 用于简化书写！！！


2. JSP原理
    * JSP本质上就是一个Servlet

3. JSP脚本：JSP定义Java代码的方式
    1. <% 代码 %>：定义的java代码，在service方法中。service方法中可以定义什么，该脚本中就可以定义什么。
    2. <%!  代码 %>：定义的java代码，在 jsp 转换后，在 java 类的成员位置
    3. <%= 代码 %>：定义的java代码，会输出到页面上。输出语句中可以定义什么，该脚本就可以定义什么。

* 示例：

```jsp
  <%
    int i = 100;
    System.out.println("定义在service方法中");
  %>

  <%!
      int i = 500;
    public void method() {
      System.out.println("可以定义成员方法或成员变量");
    }
  %>

  <%=
  // 输出到页面
    i
  %>
  hi~ jsp
  $END$
```

4. JSP的内置对象
    * 内置对象：在 jsp 页面中不需要获取和创建，可以直接使用的对象
    * jsp 一共有9个内置对象
    * 其中的三个
        * request：
        * response：
        * out：字符输出流对象。可以将数据输出到页面上。和 response.getWriter() 类似
            * response.getWriter() 和 out.writer() 的区别：
                 * 在 tomcat 服务器真正给客户端做出响应之前，会先找 response 缓冲区数据，再找 out 缓冲区数据。
                 * response.getWriter() 数据输出永远在 out.writer()之前



5. 案例：使用jsp完成cookie中的案例

```jsp
<%@ page import="java.io.UnsupportedEncodingException" %>
<%@ page import="java.net.URLDecoder" %>
<%@ page import="java.net.URLEncoder" %>
<%@ page import="java.text.SimpleDateFormat" %>
<%@ page import="java.util.Date" %>
<%@ page import="com.sun.jdi.Value" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>jsp标题</title>
</head>
<body>

<%!
    /**
     * 更新时间
     *
     * @return 格式化的时间字符串格式
     * @throws UnsupportedEncodingException
     */
    private String updateTime() throws UnsupportedEncodingException {
        // 写回cookie，更新时间
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        String str_date = sdf.format(new Date());
        // URL编码
        str_date = URLEncoder.encode(str_date, "utf-8");
        return str_date;
    }
%>
<%
    // 1.获取所有的cookie
    Cookie[] cookies = request.getCookies();
    // 2.遍历cookies
    boolean flag = false; // 没有cookie，则为false
    if (cookies != null && cookies.length > 0) {
        for (Cookie cookie : cookies) {
            // 3.获取cookie的名称
            String name = cookie.getName();
            // 4.判断名称是否是：lastTime
            if ("lastTime".equals(name)) {
                // 有该cookie，不是第一次访问
                flag = true;
                System.out.println("有cookie");
                // 响应数据
                String value = cookie.getValue();
                // URL解码
                value = URLDecoder.decode(value, "utf-8");
%>

<h1>您上次的访问时间为：<%= value %></h1>

<%
            String str_date = this.updateTime();
            cookie.setValue(str_date);
            // 设置cookie的存活时间
            //cookie.setMaxAge(0); // 删除cookie
            response.addCookie(cookie); // 发送cookie
            // 找到lastTime的cookie后，退出，不再继续循环判断
            break;
        }
    }
    if (cookies == null || cookies.length == 0 || flag == false) {
        // 没有lastTime的cookie，第一次访问
        System.out.println("没有cookie，设置cookie");
        // 响应数据
%>

<h1>您好，欢迎首次登录</h1>

<%
            String str_date = this.updateTime();
            Cookie cookie = new Cookie("lastTime", str_date);
//                System.out.println(cookie.getValue().length());
            // 设置cookie的存活时间
            cookie.setMaxAge(60 * 60 * 24 * 30); // cookie存活1个月
            response.addCookie(cookie); // 发送cookie
        }
    }
%>
</body>
</html>
```

### 2. 指令

* 作用：用于配置JSP页面，导入资源文件
* 格式：
    * <% 指令名称 属性名1=属性值1 属性名2=属性值2 ...%>
* 分类：
    1. `page`：配置JSP页面的
        * `contentType`：等同于`response.setContentType()`
            1. 设置响应体的 mime 类型以及 字符集
            2. 设置当前 jsp 页面的编码（只能是高级IDEA才能生效，如果使用低级工具，则需要设置 pageEncoding 属性设置当前页面的字符集）
        * `import`：导包
        * `errorPage`：当前页面发生异常后，会自动跳转到指定的错误页面
        * `iserrorPage`：表示当前页面是否是错误页面
            * true：是，可以使用内置对象 exception
            * false：否，默认值。不可以使用内置对象 exception
    2. `include`：页面包含的。导入页面的资源文件
        * `<%@ include file="top.jsp" %>`
    3. `taglib`：导入资源
        * `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

### 3. 注释
1. html注释：
    * <!-- -->：只能注释html代码片段
2. jsp注释：推荐使用
    * <%-- --%>：可以注释所有

### 4. 内置对象
* 在jsp页面中不需要创建，直接使用的对象
* 一共有9个内置对象：

| 变量名 |   真实类型   |  作用    |
| ------ | ---- | ---- |
| pageContext | PageContext | 当前页面共享数据，还可以获取其他八个内置对象 |
| request | HttpServletRequest | 一次请求访问的多个资源(转发) |
| session | HttpSession | 一次会话的多个请求间 |
| application | ServletContext | 所有用户间共享数据 |
| response | HttpServletResponse | 响应对象 |
| page | Object | 当前页面(Servlet)的对象，也就是 this |
| out | JspWriter | 输出对象，数据输出到页面上 |
| config | ServletConfig | Servlet的配置对象 |
| exception | Throwable | 异常对象 |



>  注：exception 对象：声明了 iserrorPage="true" 才会有
>
> ```jsp
> <%@ page contentType="text/html;charset=UTF-8" isErrorPage="true" language="java" %>
> ```


## 3. MVC：开发模式

### 1. jsp演变历史

1. 早期只有Servlet，只能使用 response 输出标签数据，非常麻烦
2. 后来有sun公司制作 jsp标准，简化了Servlet的开发，如果过度使用 jsp，在 jsp 中写了大量的 java 代码，又写html标签，造成难于维护，难于分工协作
3. 再后来，**java的web开发**，借鉴 **mvc 开发模式**，使得程序的设计更加合理性。

### 2. MVC

１. Ｍ：Model，模型。JavaBean

* 完成具体的业务操作，如：查询数据库，封装对象

２. Ｖ：View，视图。JSP

  *  展示数据

３. Ｃ：Controller，控制器。Servlet 

*   获取用户的输入

* 调用模型
* 将数据交给视图进行展示

> 优缺点：
    １. 优点：
      １. 耦合性低，方便维护，可以利于分工协作
      ２. 重用性高
    ２. 缺点：
      １. 使得项目的架构变得复杂，对开发人员要求高

## 4. EL表达式

１.	概念：Expression Language 表达式语言
２. 	作用：替换和简化 jsp 页面中 java 代码的编写
３.　语法：`${表达式}`
４.　注意：

* jsp默认支持el表达式的。如果要忽略el表达式
        1. 设置jsp中page指令中：`isELIgnored="true"`忽略当前jsp页面中所有的el表达式
            2. `\${表达式}`：忽略当前这个el表达式

5. 使用：
    1. 运算：
    *    运算符：
        1.  算数运算符：+，-， *， /(div)， %(mod)
        
        2.  比较运算符：>，<，>=，<=，==，!=
        
        3.  逻辑运算符：&&(and)，||(or)，!(not)
        
        4.  空运算符：empty
            >   功能：用于判断字符串、集合、数组对象是否为 null 或者 长度是否为 0
            
            *   `${empty list}`
            
        5.  not empty：
        
            >   功能：用于判断字符串、集合、数组对象是否不为 null 并且 长度 > 0
        
            *   `${not empty list}`
        
    2.  获取值
        1.  el 表达式只能从域对象中获取值
        
        2.  语法：
            1.  `${域名称.键名}`：从指定域中获取指定键的值
                *   域名称：
                    1.  `pageScope`		           -->	pageContext
                    2.  `requestScope`             -->     request
                    3.  `sessionScope`             -->     session
                    4.  `applicationScope`     -->     application（java中的ServletContext）
                *   举例：在request域中存储了 name=张三
                *   获取：`${requestScope.name}`
            2.   `${键名}`：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。
            *   示例：
            
            ```java
            <%
                User user = new User();
                user.setName("张三");
                user.setAge(24);
                user.setBirthday(new Date());
                request.setAttribute("u", user);
            
                List list = new ArrayList<>();
                list.add("aaa");
                list.add("bbb");
                list.add(user);
            
                request.setAttribute("list", list);
            
                Map map = new HashMap<>();
                map.put("sname", "李四");
                map.put("gender", "男");
                map.put("user", user);
            
                request.setAttribute("map", map);
            %>        
        <%--
                * 通过的对象的属性来获取
                    * setter 或 getter方法，去掉set或get，再将剩余部分，首字母变为小写
                    * setName --> Name --> name
                --%>
           <h3>el获取对象的值</h3>
            ${requestScope.u.name}<br>
            ${u.age}<br/>
            ${u.birthday}<br/>
            ${u.birthday.month}<br/>
            ${u.birStr}
           ```
        
    3.	获取对象：List集合、Map集合的值
        1.  对象：`${域名称.键名.属性名}`
            *   本质上会去调用对象的 getter 方法
        2.  List集合：`${域名称.键名[索引]}`
        
        *   例：
        
            ```java
            ${list}<br/>
            ${list[0]}<br/>
            ${list[1]}<br/>
            ${list[2].name}<br/>
             // 当数组索引越界时，不会发生异常，el 表达式内部做了处理，会显示空字符串
        ${list[10]}
            ```
        
        3.  Map集合：`${域名称.键名.key名称}` 或者 `${域名称.键名["key名称"]}`
        
        *   例：
        
            ```java
            ${map}<br/>
            ${map.sname}<br/>
            ${map["sname"]}<br/>
            ${map.gender}<br/>
            ${map.user.name}<br/>
            ${map.user["name"]}<br/>
            ```
        
    4.  隐式对象
    
        *   el 表达式中有11个隐式对象
        *   `pageContext`：
            *   获取jsp其他8个内置对象
            *   `${pageContext.request.contextPath}`：动态获取虚拟目录

## 5. JSTL标签

### 1. 概述

1.  概念：JavaServer Pages Tag Library `JSP标准标签库`
    *   是由Apache组织提供的开源的免费的 jsp 标签	<标签>
2.  作用：用于简化和替换 jsp 页面上的 java 代码
3.  使用步骤：
    1.  导入 jstl 相关的 jar 包：`jstl-1.2.jar`、`standard-1.1.2.jar`
    2.  引入标签库：taglib指令：`<%@ taglib prefix=""  ="" %>`
    3.  使用标签

### 2. 常用的JSTL标签

1.  `if`：相当于 java 代码的 `if 语句`
    1.  属性：
        *   `test`：==必须属性==，接受 boolean 表达式
            *   如果表达式为 true，则显示 if 标签体的内容
            *   如果表达式为 false，则不显示 if 标签体的内容
            *   一般情况下，test 属性值会结合 el 表达式一起使用
    2.  注意：
        *   `c:if` 标签没有else情况，想要else情况，则可以再定义一个 `c:if`标签

2.  `choose`：相当于 java 代码的 `switch 语句`
    1.  使用`choose`标签取出数据                         相当于 `switch` 的声明
    2.   使用`when`标签做判断                               相当于 `case`
    3.  使用`otherwise`标签做其他情况的声明    相当于 `default`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>choose标签</title>
</head>
<body>
<%--
    完成数字编号对应星期几的案例
        1. 域中存储数字编号
        2. 使用choose标签取出数据              相当于 switch 的声明
        3. 使用when标签做判断                  相当于 case
        4. 使用otherwise标签做其他情况的声明    相当于 default
--%>

<%
    request.setAttribute("number", 12);
%>

<c:choose>
    <c:when test="${number == 1}">星期一</c:when>
    <c:when test="${number == 2}">星期二</c:when>
    <c:when test="${number == 3}">星期三</c:when>
    <c:when test="${number == 4}">星期四</c:when>
    <c:when test="${number == 5}">星期五</c:when>
    <c:when test="${number == 6}">星期六</c:when>
    <c:when test="${number == 7}">星期日</c:when>

    <c:otherwise>输入的数字有误</c:otherwise>
</c:choose>

</body>
</html>
```

3.  `foreach`：相当于java代码的for语句
    1.  完成重复的操作
    2.  遍历容器

*   示例

```java
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>foreach标签</title>
</head>
<body>
<%--
    foreach：相当于java代码的for语句
    1. 完成重复的操作
        for(int i = 0; i < 10; i++) {

        }
        * 属性：
            begin：开始值
            end：结束值
            var：临时变量
            step：步长
            varStatus：循环状态对象
                index：容器中元素的索引，从0开始
                count：循环次数，从1开始
    2. 遍历容器
        List<User> list;
        for (User user:list) {

        }
        * 属性：
            items：容器对象
            var：容器中元素的临时变量
            varStatus：循环状态对象
                index：容器中元素的索引，从0开始
                count：循环次数，从1开始
--%>

<c:forEach begin="1" end="10" var="i" step="1">
    ${i} <br/>
</c:forEach>
<hr/>

<%
    List list = new ArrayList<>();
    list.add("aaa");
    list.add("bbb");
    list.add("ccc");
    request.setAttribute("list", list);

%>
<c:forEach items="${list}" var="str" varStatus="s">
   ${s.index} ${s.count} ${str} <br/>
</c:forEach>

</body>
</html>

```

### 3. JSTL练习

```java
<%@ page import="com.my.domain.User" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.Date" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>jstl练习</title>
</head>
<body>

<%
    /*
        需求：
            在request域中用List集合存储一些User对象。
            需要使用jstl+el将List集合数据展示到jsp页面的表格table中
     */
    List<User> list = new ArrayList<>();
    list.add(new User("张三", 20, new Date()));
    list.add(new User("李四", 24, new Date()));
    list.add(new User("王五 ", 23, new Date()));

    request.setAttribute("list", list);
%>

<table border="1" align="center" width="500">
    <tr>
        <th>编号</th>
        <th>姓名</th>
        <th>年龄</th>
        <th>生日</th>
    </tr>
    <%--展示数据--%>
    <c:forEach items="${list}" var="user" varStatus="s">
        <c:if test="${s.count % 2 == 0}">
            <tr bgcolor="red">
                <td>${s.count}</td>
                <td>${user.name}</td>
                <td>${user.age}</td>
                <td>${user.birStr}</td>
            </tr>
        </c:if>

        <c:if test="${s.count % 2 != 0}">
            <tr bgcolor="green">
                <td>${s.count}</td>
                <td>${user.name}</td>
                <td>${user.age}</td>
                <td>${user.birStr}</td>
            </tr>
        </c:if>
    </c:forEach>
</table>

</body>
</html>

```



## 6. 综合练习

## 7. Filter：过滤器



### 1. 概述

*   生活中的过滤器：净水器、空气净化器、土匪
*   web中的过滤器：当客户端访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。
*   过滤器的作用：
    *   一般用于完成通用的操作。如：登陆验证、统一编码处理、敏感字符过滤……

*   图解：

![](<https://img04.sogoucdn.com/app/a/100520146/09cf0d0a371c1495824914c059c48e7a>)


### 2. 快速入门

1.  步骤：
    1.  定义一个类，实现接口Filter
    2.  复写接口中的方法
    3.  配置拦截路径
        1.  web.xml
        2.  注解(annotation)
2.  示例：

```java
package indi.szj.web;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

/**
 * 过滤器快速入门程序
 */
@WebFilter("/*") // 访问所有资源之前，都会执行该过滤器
public class FilterDemo1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("FilterDemo1被执行了....");

        // 放行
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {

    }
}

```



### 3. 过滤器细节

#### 1. web.xml配置

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <filter>
        <filter-name>demo1</filter-name>
        <filter-class>indi.szj.web.FilterDemo1</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>demo1</filter-name>
        <!--拦截路径-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>

```



#### 2. 过滤器执行流程

1.  执行过滤器
2.  执行放行后的资源
3.  回来执行过滤器放行代码下边的代码

```java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
    // 对request对象的请求消息增强
    System.out.println("filterDemo2执行了...");
    // 放行
    chain.doFilter(req, resp);
    // 对response对象的响应消息增强
    System.out.println("filerDemo2回来了");
}
```

#### 3. 过滤器的生命周期方法

1.  init：
2.  doFilter：
3.  destory：

```java
    /**
     * 在服务器启动后，会创建Filter对象，然后调用init方法
     * 只执行一次，用于加载资源
     *
     * @param config
     * @throws ServletException
     */
    public void init(FilterConfig config) throws ServletException {
        System.out.println("init.........");
    }

    /**
     * 每一次请求资源被拦截时，会执行。
     * 执行多次
     *
     * @param req
     * @param resp
     * @param chain
     * @throws ServletException
     * @throws IOException
     */
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println("doFilter执行了...");
        chain.doFilter(req, resp);
        System.out.println("doFilter回来了...");
    }

    /**
     * 在服务器关闭后，Filter对象被销毁。
     * 如果服务器是正常关闭，则会执行destory方法
     * 只执行一次，用于释放资源
     */
    public void destroy() {
        System.out.println("destory...");
    }

```




#### 4. 过滤器的配置详解

*   拦截路径配置：
    1.  具体资源路径：`/index.jsp`：只有访问index.jsp资源时，过滤器才会被执行
    2.  拦截目录：`/user/*`：访问/user下的所有资源时，过滤器都会被执行
    3.  后缀名拦截：`*.jsp`：访问所有后缀名为jsp资源时，过滤器都会被执行
    4.  拦截所有资源：`/*`：访问所有资源时，过滤器都会被执行

*   拦截方式配置：资源被访问的方式

    *   注解配置：

        *   设置`dispatcherTypes属性`

            1.  `REQUEST`：默认值。浏览器直接请求资源
            2.  `FORWORD`：转发访问资源
            3.  `INCLUDE`：包含访问资源
            4.  `ERROR`：错误跳转资源
            5.  `ASYNC`：异步访问资源

            ```java
            //@WebFilter("/index.jsp")
            //@WebFilter("/user/*") // 拦截user目录下的所有资源
            //@WebFilter(value = "/index.jsp", dispatcherTypes = DispatcherType.REQUEST)
            @WebFilter(value = "/*", dispatcherTypes = {DispatcherType.REQUEST, DispatcherType.FORWARD})
            ```

        *   web.xml配置

            *   设置`<dispatcher></dispatcher>`标签即可

#### 5. 过滤器链（配置多个过滤器）

*   执行顺序：如果有两个过滤器：过滤器1和过滤器2
    1.  过滤器1
    2.  过滤器2
    3.  资源执行
    4.  过滤器2
    5.  过滤器1
*   过滤器先后顺序问题：
    1.  注解配置：按照`类名`的字符串比较规则，值小的先执行
        *   如：`AFilter` 和`BFilter`，`AFilter`就先执行了。
    2.  web.xml配置：`<filter-mapping></filter-mapping>`谁定义在上边，谁就先执行。



### 4. 案例1_登录验证

*   基于综合练习添加`Filter`



>   需求：
>   	1. 访问day17_case案例的资源。验证其是否登录
>   	3. 如果登录了，则直接放行。
>   	4. 如果没有登录，则跳转到登录页面，提示"您尚未登录，请先登录"。

*   图解：

![](<https://img01.sogoucdn.com/app/a/100520146/bf639079b4d02f79742c141e97a23e70>)



*   代码：

```java
package indi.szj.web.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * 登录验证的过滤器
 */
@WebFilter("/*")
public class LoginFilter implements Filter {

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        // 0.强制类型转换
        HttpServletRequest request = (HttpServletRequest) req;
        // 1.获取资源请求路径
        String uri = request.getRequestURI();
        // 2.判断是否包含登录相关资源路，要注意排除掉 css/js/图片/验证码/fonts等等
        if (uri.contains("/loginServlet") || uri.contains("/login.jsp") || uri.contains("/css/") || uri.contains("/js/") || uri.contains("/fonts/") || uri.contains("/checkCodeServlet")) {
            // 包含，用户想登录，放行
            chain.doFilter(req, resp);
        } else {
            // 不包含，需要验证用户是否登录
            // 3.从session中获取user
            Object user = request.getSession().getAttribute("user");
            if (user != null) {
                // 已经登录了，放行
                chain.doFilter(req, resp);
            } else {
                // 没有登录，跳转到登录页面
                request.setAttribute("login_msg", "您尚未登录，请登录");
                request.getRequestDispatcher("/login.jsp").forward(req, resp);
            }
        }
    }

    public void init(FilterConfig config) throws ServletException {

    }

    public void destroy() {

    }
}

```

### 5 案例2_过滤敏感词汇

>   需求：
>   	1. 对day17_case案例录入的数据进行敏感词汇过滤
>   	2. 敏感词汇：笨蛋、坏蛋
>   	3. 如果是敏感词汇，替换为 *** 



*   过滤敏感图解：

![](<https://ae01.alicdn.com/kf/Hf4b42cd9bab445828db83c6011896f3bj.bmp>)


#### 设计模式

>   为了增强对象的功能，使用设计模式
>
>   设计模式： 一些通用的解决固定问题的方式。


1.  装饰模式
2.  代理模式
    *   代理模式图解：
![](<https://ae01.alicdn.com/kf/H95b7f6fc5ede491c98ec7f6cd54cfdf6r.bmp>)

    *   概念：
        1.  真实对象：被代理的对象
        2.  代理对象
        3.  代理模式：代理对象代理真实对象，达到增强真实对象功能的目的
    *   实现方式：
        1.  `静态代理`：有一个类文件描述代理模式
        2.  `动态代理`：在内存中形成代理类
            *   实现步骤：
                1.  `代理对象和真实对象实现相同的接口`
                2.  `代理对象 = Proxy.newProxyInstance();`
                3.  `使用代理对象调用方法`
                4.  `增强方法`
            *   增强方式：
                1.  增强参数列表
                2.  增强返回值
                3.  增强方法体执行逻辑

*   示例：

```java
package indi.szj.proxy;

public interface SaleComputer {

    public abstract String sale(double money);

    public abstract void show();
}
```



```java
package indi.szj.proxy;

/**
 * 真实类
 */
public class Lenovo implements SaleComputer {
    @Override
    public String sale(double money) {
        System.out.println("买了" + money + "元的电脑");
        return "联想电脑";
    }

    @Override
    public void show() {
        System.out.println("展示电脑。。。");
    }
}
```



```java
package indi.szj.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {
    public static void main(String[] args) {
        // 1.创建真实对象
        Lenovo lenovo = new Lenovo();
        // 2.动态代理增强lenovo对象
        /*
            三个参数：
                1. 类加载器：真实对象.getClass().getClassLoader()
                2. 接口数组：真实对象.getClass().getInterfaces()
                3. 处理器：new InvocationHandler()
         */
        SaleComputer proxy_lenovo = (SaleComputer) Proxy.newProxyInstance(lenovo.getClass().getClassLoader(), lenovo.getClass().getInterfaces(), new InvocationHandler() {
            /*
                代理逻辑编写的方法：代理对象调用的所有方法都会触发该方法的执行
                参数：
                    1. proxy:代理对象
                    2. method:代理对象调用的方法，被封装为的对象
                    3. args:代理对象调用的方法时，传递的实际参数
             */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                /*System.out.println("该方法执行了...");
                System.out.println(method.getName());
                System.out.println(args[0]);*/

                // 判断是否是sale方法
                if ("sale".equals(method.getName())) {
                    // 1.增强参数
                    double money = (double) args[0];
                    money = money * 0.85;
                    // 2.增强方法体
                    System.out.println("专车接你。。。");
                    // 使用真实对象调用该方法
                    String obj = (String) method.invoke(lenovo, money);
                    // 2.增强返回值
                    System.out.println("免费送货。。。");
                    return obj + "_鼠标垫";
                } else {
                    Object obj = method.invoke(lenovo, args);
                    return obj;
                }
            }
        });

        String computer = proxy_lenovo.sale(8000);
        System.out.println(computer);
        proxy_lenovo.show();
    }
}

```

