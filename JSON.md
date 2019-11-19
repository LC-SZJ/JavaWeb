[TOC]

# JSON

## 1.概述

1. 概念： `JavaScript Object Notation`		JavaScript对象表示法

*   Java中封装对象

```java
Person p = new Person();
p.setName("张三");
p.setAge(23);
p.setGender("男");
```

*   Js中封装对象

```javascript
var p = {"name":"张三","age":23,"gender":"男"};
```




>   *   JSON早期是Javascript用来封装对象的。
>
>   -   json现在多用于**存储和交换文本信息的语法**，类似于XML。
>   -   进行数据的传输。
>   -   JSON 比 XML 更小、更快，更易解析。



## 2.语法

### 1.基本规则

*   数据在`名称/值`对中：json数据是由**键值对**构成的
    *   **键用引号（单双都行）引起来，也可以不使用引号**
    *   值的的取值类型：
        1.  `数字`（整数或浮点数）
        2.  `字符串`（在双引号中）
        3.  `逻辑值`（true 或 false）
        4.  `数组`（在方括号中）`{"person":[{},{}]}`
        5.  `对象`（在花括号中）`{"address":{"province":"陕西"...}}`
        6.  `null`
*   **数据由逗号分隔**：多个键值对由逗号 `,`分隔
*   **花括号保存对象**：使用`{ }`定义 json 格式
*   **方括号保存数组**：`[ ]`



*   示例：

```javascript
    // 1.定义基本格式
    var person = {"name": "张三", "age": 20, "gender": true};

    // 2.嵌套格式  {} --> []
    var persons = {
        "persons": [
            {"name": "张三", "age": 23, "gender": true},
            {"name": "李四", "age": 22, "gender": true},
            {"name": "王五", "age": 25, "gender": false}
        ]
    };

   // 3.嵌套格式  [] --> {}
   var ps = [{"name": "张三", "age": 23, "gender": true},
       {"name": "李四", "age": 22, "gender": true},
       {"name": "王五", "age": 24, "gender": false}
   ];
```



### 2.获取数据

1.  `json对象.键名`
2.  `json对象["键名"]`
3.  `数组对象[索引]`

*   示例1：

```javascript
 // 1.定义基本格式
   var person = {"name": "张三", "age": 20, "gender": true};
   // 获取person中的name值
   // json对象.键名
   var name = person.name;
   // json对象["键名"]
   var name = person["name"];

   // 2.嵌套格式  {} --> []
   var persons = {
      "persons": [
          {"name": "张三", "age": 23, "gender": true},
          {"name": "李四", "age": 22, "gender": true},
          {"name": "王五", "age": 25, "gender": false}
      ]
  };
  // 获取张三的值
  var name1 = persons.persons[0].name;

   // 3.嵌套格式  [] --> {}
   var ps = [{"name": "张三", "age": 23, "gender": true},
       {"name": "李四", "age": 22, "gender": true},
       {"name": "王五", "age": 24, "gender": false}
   ];
   // 获取李四的age
   var name2 = ps[1].age;
```



*   示例2：

```javascript
// 1. 单个对象 
var person = {"name": "张三", "age": 20, "gender": true};
 // 循环遍历person中的key
 for (var key in person) {
     // 这样是获取不到的，类似于person."name"
     // 获取到的key默认是字符串
     // alert(key + ":" + person.key);
     // 这样才可以
     alert(key + ":" + person[key]);
 }

// 2. 数组对象
 var ps = [{"name": "张三", "age": 23, "gender": true},
     {"name": "李四", "age": 22, "gender": true},
     {"name": "王五", "age": 24, "gender": false}
 ];
 // 获取ps中的所有值
 for (var i = 0; i < ps.length; i++) {
     var p = ps[i];
     for (var key in p) {
         // alert(key + ":" + p[key]);
     }
 }

// 3. 对象的值是数组
var persons = {
    "persons": [
        {"name": "张三", "age": 23, "gender": true},
        {"name": "李四", "age": 22, "gender": true},
        {"name": "王五", "age": 25, "gender": false}
    ]
};

for (var keys in persons) {
    var arr = persons[keys];
    for (var i = 0; i < arr.length; i++) {
        var p = arr[i];
        for (var key in p) {
            alert(key + ":" + p[key]);
        }
    }
}
```



### 3.JSON数据和Java对象的相互转换



>   JSON解析器：
>
>   *   常见的解析器：Jsonlib、Gson、fastjson（阿里）、jackson（spring mvc 框架的内置对象）



#### 1.JSON转为Java对象

*   使用步骤

    1. 导入jackson的相关jar包
    2. 创建Jackson核心对象 ObjectMapper
    3. 调用ObjectMapper的相关方法进行转换
          1. `readValue(json字符串数据,Class)`

*   简单示例：

```java
// 演示 JSON字符串转为Java对象
@Test
public void test4() throws IOException {
    // 1.初始化字符串
    String json = "{\"gender\":\"男\",\"name\":\"张三\",\"age\":22}";
    // 2.创建ObjectMapper对象
    ObjectMapper mapper = new ObjectMapper();
    Person person = mapper.readValue(json, Person.class);
    System.out.println(person);
}
```



#### 2.Java对象转换JSON

* 使用步骤：

    1.  导入Jackson的相关jar包
        1.  `jackson-annotations-2.2.3.jar`
        2.  `jackson-databind-2.2.3.jar`
        3.  `jackson-core-2.2.3.jar`
    2.  创建Jackson核心对象 `ObjectMapper`
    3.  调用`ObjectMapper`的相关方法进行转换

    *   转换方法：

        *   `writeValue(参数1, obj)`   
        
        | 参数1类型    | 作用                                                     |
        | ------------ | -------------------------------------------------------- |
        | File         | 将obj对象转换为JSON字符串,并保存到指定文件中             |
        | Writer       | 将obj对象转换为JSON字符串,并将json数据填充到字符输出流中 |
        | OutputStream | 将obj对象转换为JSON字符串,并将json数据填充到字节输出流中 |
        
        *   `writeValueAsString(obj)`：将json对象转为字符串
        
    *   注解：
    
        1.  `@JsonIgnore`：排除属性
        2.  `@JsonFormat`：属性值的格式化
        
    *   复杂java对象转换
    
        1.  `List`：数组
        2.  `Map`：与对象格式一致



*   注解示例：

```java
  // @JsonIgnore
  @JsonFormat(pattern = "yyyy-MM-dd")
  private Date birthday;
```



*   List集合转JSON

```java
 Person p = new Person();
 p.setName("张三");
 p.setAge(23);
 p.setGender("男");
 p.setBirthday(new Date());

 Person p1 = new Person();
 p1.setName("张三");
 p1.setAge(23);
 p1.setGender("男");
 p1.setBirthday(new Date());

 List<Person> list = new ArrayList<Person>();
 list.add(p);
 list.add(p1);

 ObjectMapper mapper = new ObjectMapper();
 String json = mapper.writeValueAsString(list);
 System.out.println(json);
 // [{"name":"张三","age":23,"gender":"男","birthday":"2019-11-18"},{"name":"张三","age":23,"gender":"男","birthday":"2019-11-18"}]
```



*   Map集合转JSON

```java
 Map<String, Object> map = new HashMap<String, Object>();
 map.put("name", "张三");
 map.put("age", 22);
 map.put("gender", "男");

 ObjectMapper mapper = new ObjectMapper();
 String json = mapper.writeValueAsString(map);
 System.out.println(json);
// {"gender":"男","name":"张三","age":22}
```



*   Java对象转换JSON示例：

```java
 // 1.创建对象
  Person p = new Person();
  p.setName("张三");
  p.setAge(23);
  p.setGender("男");

  // 2.创建JSON核心对象：ObjectMapper
  ObjectMapper mapper = new ObjectMapper();
  // 3.转换
  String json = mapper.writeValueAsString(p);
  System.out.println(json);
  // writeValue,将数据写到d://a.txt文件中
  mapper.writeValue(new File("D://a.txt"), p);
  // writeValue,将数据关联到Write流中
  mapper.writeValue(new FileWriter("D://b.txt"), p);
```



## 3.案例：校验用户名是否存在

*   前台代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
    <script src="js/jquery-3.3.1.min.js"></script>
    <script>
        // 入口函数,也就是在页面加载完成后，相当于window.onload() = function(){}
        $(function () {
            // 给username绑定离焦事件
            $("#username").blur(function () {
                // 获取username文本输入框的值
                var username = $(this).val();
                // 发送ajax请求
                /*
                   期望服务器响应回的数据格式：{"userExsit":true,"msg":"此用户名太受欢迎,请更换一个"}
                                            {"userExsit":false,"msg":"用户名可用"}
                 */
                $.get("findUserServlet", {username: username}, function (data) {
                    // 判断 userExist键是否是true
                    var span = $("#s_username");
                    if (data.userExist) {
                        // 用户名存在
                        span.css("color", "red");
                        span.html(data.msg);
                    } else {
                        // 用户名不存在
                        span.css("color", "green");
                        span.html(data.msg);
                    }

                }); // 如果 type不写"json",默认是"text"类型，会将data当作字符串
            });
        });

    </script>
</head>
<body>
<form action="findUserServlet" method="get">
    <input type="text" id="username" name="username" placeholder="请输入用户名">
    <span id="s_username"></span>
    <br/>
    <input type="password" name="password" placeholder="请输入密码"><br/>

    <input type="submit" value="注册"><br/>
</form>

</body>
</html>
```



*   后台代码

```java
package indi.szj.web.servlet;

import com.fasterxml.jackson.databind.ObjectMapper;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

@WebServlet("/findUserServlet")
public class FindUserServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 指定响应流的编码格式
        // response.setContentType("text/html;charset=utf-8");

        // 第二种方式：设置响应的数据格式为：json
        response.setContentType("application/json;charset=utf-8");

        // 获取username参数
        String username = request.getParameter("username");
        // 调用service层，判断用户名是否存在,这里写死了
        Map<String, Object> map = new HashMap<String, Object>();
        if ("tom".equals(username)) {
            // 用户名存在
            map.put("userExist", true);
            map.put("msg", "此用户名太受欢迎了，请更换一个");
        } else {
            // 用户名不存在
            map.put("userExist", false);
            map.put("msg", "用户名可用");
        }

        // 创建json核心对象
        ObjectMapper mapper = new ObjectMapper();
        // 将map转为json，并且传递给客户端
        mapper.writeValue(response.getWriter(), map);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

