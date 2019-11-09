[TOC]

# JDBC

## 1. 概述

- 概念：Java Database Connectivity Java数据库连接，Java语言操作数据库
- JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去分别实现这套接口，提供数据库驱动 jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动 jar包中的实现类。

## 2.快速入门

- 步骤：
    1. 导入驱动 jar包 mysql-connector-java-5.1.48-bin.jar
        1. 复制mysql-connector-java-5.1.48-bin.jar到项目的 libs(可以是别的) 目录下
        2. 右键 --> Add as Library
    2. 注册驱动
    3. 获取数据库连接对象：Connection
    4. 定义 sql
    5. 获取执行 sql语句的对象：Statement
    6. 执行 sql，接受返回结果
    7. 处理结果
    8. 释放资源
- 示例：

    ```java
    package com.my.jdbc;
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.Statement;

    /**
     * JDBC快速入门
     */
    public class JdbcDemo1 {
        public static void main(String[] args) throws Exception {
            // 1.导入驱动 jar包
            // 2.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 3.获取数据库连接对象
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
            // 4.定义sql语句
            String sql = "update account set banlance = 500 where id = 1";
            // 5.获取执行sql的对象 Statement
            Statement stmt = conn.createStatement();
            // 6.执行sql
            int count = stmt.executeUpdate(sql);
            // 7.处理结果
            System.out.println(count);
            // 8.释放资源
            stmt.close();
            conn.close();
        }
    }
    ```
    
## 3.详解各个对象：

### 1.DriverManager：驱动管理对象
**功能**：

#### 1.注册驱动:告诉程序该使用哪一个数据库的驱动jar包
```java
// 注册与给定的驱动程序 DriverManager
static void registerDriver(Driver driver) 
```
写代码时使用：
```java
Class.forName("com.mysql.jdbc.Driver");
```
通过查看源码发现：在 com.my.sql.jdbc.Driver类中存在静态代码块

```mysql
static {
    try {
        java.sql.DriverManager.registerDriver(new Driver());
    } catch (SQLException E) {
        throw new RuntimeException("Can't register driver!");
    }
}
```
> 注意：mysql 5 之后的驱动 jar包可以省略注册驱动的步骤
> 

#### 2.获取数据库连接：
* 方法：
```java
static Connection getConnection(String url, String user, String password) 
// 尝试建立与给定数据库URL的连接。  
```
* 参数：
    * url：指定连接的路径
        * 语法:
        ```mysql
        jdbc:mysql://ip地址(域名)/端口/数据库名称
        ```
        * 例子:
        ```mysql
        jdbc:mysql://localhost/3306/db3
        ```
        * 细节:如果连接的是本机的mysql服务器，并且mysql服务的端口号是3306，则url可以简写为：`jdbc:mysql:///数据库名称`
    * user：用户名
    * password：密码



### 2.Connection：数据库连接对象

**功能**：
#### 1.获取执行 sql 的对象
- Statement createStatement()  
- PreparedStatement prepareStatement(String sql)  

#### 2.管理事务

- 开启事务：setAutoCommit(boolean autoCommit):调用该方法设置参数为 false，即开启事务
- 提交事务：commit() 
- 回滚：rollback() 

### 3.Statement：执行sql的对象

#### 1.执行sql
1. boolean execute(String sql)：可以执行任意的 sql(了解)
2. int executeUpdate(String sql)：执行DML(insert、update、delete)语句、DDL(create、alter、drop)语句
    * 返回值：影响的行数，可以通过这个影响的行数判读DML语句是否执行成功，如果返回值 > 0，则执行成功，反之，失败。
3. ResultSet executeQuery(String sql)：执行DQL(select)语句

#### 2.练习

##### DML语句

```java
package com.my.jdbc;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * DML语句：
 * account表：
 * 添加一条记录 --> insert 语句
 * 修改一条记录 --> update 语句
 * 删除一条记录 --> delete 语句
 */
public class JdbcDemo2 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            // 1.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 2.定义sql语句
            // 添加语句
            // String sql = "insert into account values(null,'王五',3000)";
            // 修改语句
            // String sql = "update account set banlance = banlance + 1000 where id = 1";
            // 删除语句
            String sql = "delete from account where id = 2";
            // 3.获取Connection对象
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
            // 4.获取执行sql对象的 Statement对象
            stmt = conn.createStatement();
            // 5.执行sql
            int count = stmt.executeUpdate(sql);
            // 6.处理结果
            System.out.println("共影响" + count + "行");
            if (count > 0) {
                System.out.println("执行成功！");
            } else {
                System.out.println("执行失败！");
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7.释放资源
            // 避免空指针异常
            // stmt.close();
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

##### DDL语句

```java
package com.my.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * DDL语句
 * 创建student表
 */
public class JdbcDemo3 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            // 1.注册数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 2.定义sql语句
            String sql = "create table student(id int, name varchar(20))";
            // 3.获取数据库连接对象
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
            // 4.获取执行sql的对象
            stmt = conn.createStatement();
            // 5.执行sql
            int count = stmt.executeUpdate(sql);
            // 6.处理结果
            System.out.println(count); // 0 DDL语句不返回影响的行数
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7.释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```


### 4.ResultSet：结果集对象，封装查询结果

#### 1.基本使用

- boolean next():游标向下移动一行,判断当前行是否是最后一行末尾（是否有数据），如果是，则返回 false，否则返回 true
- getXxx(参数)：获取数据
    - Xxx：代表数据类型 如 int getInt(), String getString()
    - 参数：
        1. int:代表列的编号，**从1开始** 如 getString(1)
        2. String:代表列的名称 如 getDouble("balance")
- 使用步骤：
    1. 游标向下移动一行
    2. 使用循环，用 next 方法判断是否有数据
    3. 获取数据

#### 2.示例

```java
package com.my.jdbc;
import java.sql.*;
public class JdbcDemo4 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");

            String sql = "select * from account";
            stmt = conn.createStatement();
            // 返回一个结果集对象
            rs = stmt.executeQuery(sql);
            // 使游标向下移动一行
            // 循环判断游标是否是最后一行之后
            while (rs.next()) {
                int id = rs.getInt(1);
                String name = rs.getString("name");
                double banlance = rs.getDouble(3);
                System.out.println(id + "---" + name + "---" + banlance);
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

#### 3.练习

- EMP.java

```java
package com.my.domain;
import java.util.Date;
/**
 * 封装EMP表数据的JavaBean
 */
public class EMP {
    private int id;
    private String ename;
    private int job_id;
    private int mgr;
    private Date joindate;
    private double salary;
    private double bonus;
    private int dept_id;

    @Override
    public String toString() {
        return "EMP{" +
                "id=" + id +
                ", ename='" + ename + '\'' +
                ", job_id=" + job_id +
                ", mgr=" + mgr +
                ", joindate=" + joindate +
                ", salary=" + salary +
                ", bonus=" + bonus +
                ", dept_id=" + dept_id +
                '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getEname() {
        return ename;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public int getJob_id() {
        return job_id;
    }

    public void setJob_id(int job_id) {
        this.job_id = job_id;
    }

    public int getMgr() {
        return mgr;
    }

    public void setMgr(int mgr) {
        this.mgr = mgr;
    }

    public Date getJoindate() {
        return joindate;
    }

    public void setJoindate(Date joindate) {
        this.joindate = joindate;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public double getBonus() {
        return bonus;
    }

    public void setBonus(double bonus) {
        this.bonus = bonus;
    }

    public int getDept_id() {
        return dept_id;
    }

    public void setDept_id(int dept_id) {
        this.dept_id = dept_id;
    }
}
```

- DemoEMP.java

```java
package com.my.jdbc;

import com.my.domain.EMP;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/**
 * 需求：
 *      定义一个方法，查询EMP表中的数据，将其封装成对象，然后装载入集合，并返回
 */
public class DemoEMP {
    public static void main(String[] args) {
        List<EMP> list = new DemoEMP().findAll();
        list.stream().forEach(System.out::println);
//        System.out.println(list);
    }

    /**
     * 查询所有的对象的方法
     *
     * @return list集合
     */
    public List<EMP> findAll() {
        List<EMP> list = new ArrayList<>();
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            // 1.注册数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 2.获取数据库连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root","root");
            // 3.定义sql
            String sql = "select * from emp";
            // 4.获取执行sql的对象
            stmt = conn.createStatement();
            // 5.执行sql
            rs = stmt.executeQuery(sql);
            // 6.取出数据，装载进集合
            // 创建emp对象，复用引用
            EMP emp = null;
            while (rs.next()) {
                // 获取数据
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                double bonus = rs.getDouble("bonus");
                int dept_id = rs.getInt("dept_id");

                // 封装成对象
                emp = new EMP();
                emp.setId(id);
                emp.setEname(ename);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);

                // 将对象载入集合
                list.add(emp);
            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

        }
        return list;
    }
}
```



### 5.preparedStatement:执行sql的对象

#### 1.SQL注入问题
- 在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题。
- 输入用户随便，输入密码：
    ```java
    a' or 'a' = 'a;
    ```
- 输出的sql拼装后的语句:
    ```mysql
    select * from user where username = 'jkdfj' and password = 'a' or 'a' = 'a'
    ```
#### 2.解决sql注入问题

使用 **preparedStatement 对象**来解决

#### 3.预编译的SQL

参数使用 **?** 作为占位符

#### 4.使用步骤

1. 导入驱动 jar包 mysql-connector-java-5.1.48-bin.jar
    1. 复制mysql-connector-java-5.1.48-bin.jar到项目的 libs(可以是别的) 目录下
    2. 右键 --> Add as Library
    
2. 注册驱动

3. 获取数据库连接对象：Connection

4. 定义 sql
    * **注意**：sql的参数使用 **?** 作为占位符。
    * 如：
    ```mysql
    select * from user where username = ? and password = ?;
    ```

5. 获取执行 sql语句的对象：**PreparedStatement prepareStatement(String sql)  **

6. 给 **?** 赋值：
    * **方法**：setXxx(参数1,参数2)
        * 参数1:**?** 的位置编号，从1开始
        * 参数2:**?** 的值

7. 执行 sql，接受返回结果，**不需要传递 sql 语句**

8. 处理结果

9. 释放资源


> 注意：后期都会使用 **PreparedStatement** 来完成增删改查的所有操作
> 1. 可以防止SQL注入
> 2. 效率更高
> 


#### 5.示例(防止sql注入)

```java
package com.my.jdbc;

import com.my.util.JDBCUtils;
import java.sql.*;
import java.util.Scanner;

/**
 * 需求：
 * 1.创建user数据库表，有id,username,password字段,增添几条记录
 * 2.通过键盘录入用户名和密码
 * 3.判断用户是否登录成功
 */
public class JdbcPractise {

    public static void main(String[] args) {
        // 1.键盘录入，接受用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        // 2.调用方法
        JdbcPractise obj = new JdbcPractise();
        boolean flag = obj.login2(username, password);
        //3.判断结果，输出不同语句
        if (flag == true) {
            System.out.println("登录成功！");
        } else {
            System.out.println("用户名或密码错误！");
        }
    }

    /**
     * 登录方法,使用PreparedStatement实现
     */
    public boolean login2(String username, String password) {
        if (username == null || password == null) {
            return false;
        }
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            // 1.连接数据库
            conn = JDBCUtils.getConnection();
            // 2.定义sql
            String sql = "select * from user where username = ? and password = ?";
            // 3.获取执行sql的对象
            pstmt = conn.prepareStatement(sql);
            // 注意：要给 ? 赋值
            pstmt.setString(1, username);
            pstmt.setString(2, password);
            // 4.执行sql
            rs = pstmt.executeQuery();
            // 5.判断
            return rs.next();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 6.释放资源
            JDBCUtils.close(rs, pstmt, conn);
        }
        return false;
    }
}
```





### 6.创建自定义的JDBCUtils工具类

#### 1.jdbc.properties 配置文件

```java
url=jdbc:mysql:///db3
user=root
password=root
driver=com.mysql.jdbc.Driver
```

#### 2.JDBCUtils.java

```java
package com.my.util;
import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.sql.*;
import java.util.Properties;

/**
 * JDBC工具类
 */
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;

    /**
     * 文件的读取，只需要读取一次即可拿到这些值，使用静态代码块
     */
    // 静态代码块中的异常，只能捕获异常，不能抛出异常，方法中才能抛出异常
    static {
        // 读取资源文件，获取值
        try {
            // 1.创建Properties集合类
            Properties prop = new Properties();
            // 2.加载进内存
            // 获取 src路径下文件的方式 --> ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            // 统一资源定位符 URL用来定位在src路径下指定文件的绝对路径
            URL res = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
            System.out.println(path); // D:/itcast-JavaWeb/out/production/day04_jdbc/jdbc.properties/

            // prop.load(new FileReader("src/jdbc.properties"));
            prop.load(new FileReader(path));
            // 3.获取属性，赋值
            url = prop.getProperty("url");
            user = prop.getProperty("user");
            password = prop.getProperty("password");
            driver = prop.getProperty("driver");
            // 4.注册驱动
            Class.forName(driver);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     *
     * @return 连接对象
     */
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, user, password);
    }

    /**
     * 释放资源
     *
     * @param stmt
     * @param conn
     */
    public static void close(Statement stmt, Connection conn) {
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 释放资源
     *
     * @param rs
     * @param stmt
     * @param conn
     */
    public static void close(ResultSet rs, Statement stmt, Connection conn) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```


#### 3.DemoEMP.java

```java
package com.my.jdbc;

import com.my.domain.EMP;
import com.my.util.JDBCUtils;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/**
 * 需求：
 * 定义一个方法，查询EMP表中的数据，将其封装成对象，然后装载入集合，并返回
 */
public class DemoEMP {
    public static void main(String[] args) {
        List<EMP> list = new DemoEMP().findAll();
        list.stream().forEach(System.out::println);
//        System.out.println(list);
    }

    /**
     * 查询所有的对象的方法
     *
     * @return list集合
     */
    public List<EMP> findAll() {
        List<EMP> list = new ArrayList<>();
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            // 1.注册数据库驱动
            // 2.获取数据库连接
            conn = JDBCUtils.getConnection();
            // 3.定义sql
            String sql = "select * from emp";
            // 4.获取执行sql的对象
            stmt = conn.createStatement();
            // 5.执行sql
            rs = stmt.executeQuery(sql);
            // 6.取出数据，装载进集合
            // 创建emp对象，复用引用
            EMP emp = null;
            while (rs.next()) {
                // 获取数据
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                double bonus = rs.getDouble("bonus");
                int dept_id = rs.getInt("dept_id");

                // 封装成对象
                emp = new EMP();
                emp.setId(id);
                emp.setEname(ename);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);

                // 将对象载入集合
                list.add(emp);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.close(rs, stmt, conn);
        }
        return list;
    }
}
```

#### 4.练习

- jdbc.properties配置文件

```java
// 修改配置文件
url=jdbc:mysql:///db4
user=root
password=root
driver=com.mysql.jdbc.Driver
```
- 数据库表：user

```mysql
-- 创建表user
CREATE DATABASE IF NOT EXISTS db4;
CREATE TABLE USER(
	id INT PRIMARY KEY AUTO_INCREMENT,
	username VARCHAR(32),
	PASSWORD VARCHAR(32)
);
SELECT * FROM USER;

INSERT INTO USER VALUES(NULL,'zhangsan','123');
INSERT INTO USER VALUES(NULL,'lisi','456');
```

- JdbcPractise.java

```java
package com.my.jdbc;

import com.my.util.JDBCUtils;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Scanner;

/**
 * 需求：
 * 1.创建user数据库表，有id,username,password字段,增添几条记录
 * 2.通过键盘录入用户名和密码
 * 3.判断用户是否登录成功
 * 	3.1 select * from user where username = '' and password = '';
 如果这个sql有查询结果，则成功，反之，则失败。
 */
public class JdbcPractise {

    public static void main(String[] args) {
        // 1.键盘录入，接受用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        // 2.调用方法
        JdbcPractise obj = new JdbcPractise();
        boolean flag = obj.login(username, password);
        //3.判断结果，输出不同语句
        if (flag == true) {
            System.out.println("登录成功！");
        } else {
            System.out.println("用户名或密码错误！");
        }
    }

    /**
     * 登录方法
     */
    public boolean login(String username, String password) {
        if (username == null || password == null) {
            return false;
        }
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            // 1.连接数据库
            conn = JDBCUtils.getConnection();
            // 2.定义sql，拼接字符串
            String sql = "select * from user where username = '" + username + "' and password = '" + password + "'";
            // 3.获取执行sql的对象
            stmt = conn.createStatement();
            // 4.执行sql
            rs = stmt.executeQuery(sql);
            // 5.判断
            return rs.next();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 6.释放资源
            JDBCUtils.close(rs, stmt, conn);
        }
        return false;
    }
}
```

## 4.JDBC控制事务

### 1.概述

1. 事务：一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。
2. 操作
    1. 开启事务
    2. 提交事务
    3. 回滚事务
3. 使用 Connection 对象来管理事务
    1. **开启事务**：**setAutoCommit(boolean autoCommit)**:调用该方法，设置参数为 false，即开启**手动提交事务**
        - **在执行 sql 之前开启事务**
    2. **提交事务**：**commit()**
        - **当所有 sql 都执行完成**

    3. **回滚事务**：**rollback()**
- **在 catch 中回滚事务**

### 2.实现

```java
package com.my.jdbc;

import com.my.util.JDBCUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

/**
 * 转账事务操作
 */
public class JdbcDemo5 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement stmt1 = null;
        PreparedStatement stmt2 = null;

        try {
            // 获取数据库连接对象
            conn = JDBCUtils.getConnection();
            // 开启事务
            conn.setAutoCommit(false);
            // 定义sql
            // 张三 -500
            String sqlZ = "update account set banlance = banlance - ? where id = ?";
            // 李四 +500
            String sqlL = "update account set banlance = banlance + ? where id = ?";
            // 获取执行sql的 PreparedStatement 对象
            stmt1 = conn.prepareStatement(sqlZ);
            stmt2 = conn.prepareStatement(sqlL);
            // 设置参数，即给 ? 赋值
            stmt1.setDouble(1, 500);
            stmt1.setInt(2, 1);
            stmt2.setDouble(1, 500);
            stmt2.setInt(2, 2);
            // 执行sql
            stmt1.executeUpdate();
            // 手动制造异常
            int i = 3/0;
            stmt2.executeUpdate();
            // 提交事务
            conn.commit();

        } catch (Exception e) {
            try {
                // 如果 conn != null
                if (conn != null) {
                    // 回滚事务
                    conn.rollback();
                }
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            e.printStackTrace();
        } finally {
            // 释放资源
            JDBCUtils.close(stmt1, conn);
            // 不合符规范的写法
            JDBCUtils.close(stmt2, null);
        }
    }
}
```