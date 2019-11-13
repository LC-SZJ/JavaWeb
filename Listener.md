[TOC]

## Listener：监听器

### 1. Listener概述



>   web的三大组件之一
>
>   Servlet、Filter、Listener

*   事件监听机制
    *   事件：一件事情
    *   事件源：事件发生的地方
    *   监听器：一个对象
    *   注册监听：将事件、事件源、监听器绑定在一起。当事件源上发生某个事件后，执行监听器代码

### 2.ServletContextListener



>   ServletContextListener：监听ServletContext对象的创建和销毁

*    方法：

| 返回值  | 方法名                                        | 作用                                   |
| ------- | --------------------------------------------- | -------------------------------------- |
| ` void` | `contextInitialized(ServletContextEvent sce)` | ServletContext对象创建后会调用该方法   |
| `void`  | `contextDestroyed(ServletContextEvent sce)`   | ServletContext对象销毁之前会调用该方法 |

*   步骤：

    1.  定义一个类，实现`ServletContextListener`接口

    2.  复写方法

    3.  配置

        1.  `web.xml`

            ```xml
            <?xml version="1.0" encoding="UTF-8"?>
            <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
                     version="3.1">
                <!--配置监听器(注册监听)-->
                <listener>
                    <listener-class>indi.szj.web.listener.ContextLoaderListener</listener-class>
                </listener>
            
                <!--指定初始化参数-->
                <context-param>
                    <param-name>contextConfigLocation</param-name>
                    <param-value>/WEB-INF/classes/applicationContext.xml</param-value>
                </context-param>
            
            
            </web-app>
            
            ```

        2.  `注解`：`@webListener`

-   示例：

```java
package indi.szj.web.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import java.io.FileInputStream;
import java.io.FileNotFoundException;

@WebListener
public class ContextLoaderListener implements ServletContextListener {

    /**
     * 监听ServletContext对象的创建。
     * ServletContext对象会在服务器启动后自动创建
     * 这个方法会在服务器启动后自动调用
     *
     * @param servletContextEvent
     */
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        //一般用于 加载资源文件
        // 1.获取ServletContext对象
        ServletContext servletContext = servletContextEvent.getServletContext();
        // 2.加载资源
        String contextConfigLocation = servletContext.getInitParameter("contextConfigLocation");
        // 3.获取真实路径
        String realPath = servletContext.getRealPath(contextConfigLocation);
        try {
            // 4.加载进内存
            FileInputStream fis = new FileInputStream(realPath);
            System.out.println(fis);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        System.out.println("ServletContext对象被创建了...");
    }

    /**
     * 在服务器关闭后，ServletContext对象被销毁。
     * 当服务器正常关闭后，该方法自动被调用
     *
     * @param servletContextEvent
     */
    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("ServletContext对象被销毁了...");
    }
}
```

