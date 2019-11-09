[TOC]

# XML

## 1.概述

1. **概念**：Extensible Markup Language ***可扩展标记语言***
    * 可扩展：标签都是自定义的。` <user>、<student>`

2. **功能**
    * **存储数据**
        1. ***配置文件***
        2. ***在网络中传输***

3. **xml 与 html 的区别**
    1. xml 标签都是自定义的，html标签是预定义的。
    2. xml 标签语法严格，html 语法松散。
    3. xml 是存储数据的，html 是展示数据的

* w3c：xml 和 html 都来自 w3c：万维网联盟

## 2.语法

### 1.基本语法

1. **xml 文档的后缀名：`.xml`**
2. **xml 第一行必须定义为 文档声明**
3. **xml 文档中有且仅有一个根标签**
4. **属性值必须使用引号(单双都可)引起来**
5. **标签必须正确关闭**
6. **xml 标签名称区分大小写**
### 2.快速入门

```xml
<?xml version='1.0' ?>

<users>

    <user id='1'>
        <name>zhangsan</name>
        <age>23</age>
        <gender>male</gender>
    </user>

    <user id='2'>
        <name>lisi</name>
        <age>24</age>
        <gender>female</gender>
    </user>

</users>
```

### 3.组成部分:
1. **文档声明**
    1. 格式：`<?xml 属性列表 ?>`
    2. **属性列表：**
        * `version`：版本号。必须的属性
        * `encoding`：编码方式。告知解析引擎当前文档使用的字符集，默认值：ISO-8859-1
        * `standalone`：是否独立
            * 取值：
                * yes：不依赖其他文件
                * no：依赖其他文件

2. 指令(了解)：结合css的
    * `<?xml-stylesheet type="text/css" href="a.css" ?>`

3. 标签：标签名称自定义的
    * 规则：
        * 名称可以包含字母、数字以及其他字符
        * 名称不能以数字或者标点符号开始
        * 名称不能以字母 xml（或者 XML、Xml 等等）开始
        * 名称不能包含空格
4. 属性：
    * id 属性值唯一

5. 文本
    * **CDATA区：在该区域中的数据会被原样显示**
    * **格式**：`<![CDATA[ 数据 ]]>`


- 示例：

```xml
<?xml version='1.0' encoding="gbk" standalone="yes" ?>
<!--<?xml-stylesheet type="text/css" href="a.css" ?>-->


<users>

    <user id='1'>
        <name>张三</name>
        <age>23</age>
        <gender>male</gender>
    </user>

    <user id='2'>
        <name>lisi</name>
        <age>24</age>
        <gender>female</gender>
    </user>

    <code>

        <!--if (a &lt; b &amp;&amp; a &gt; c) {}-->
        <![CDATA[
        if (a > b && a < c) {}
]]>

    </code>

</users>
```

## 3.约束

### 1.概念

- 谁去编写 xml 文档？程序员
- 谁去解析 xml 文档？框架(半成品软件)

* 约束(约束文档)：规定 xml 文档的书写规则
    * 作为框架的使用者(程序员)：
        1. 能够在 xml 中引入约束文档
        2. 能够简单的读懂约束文档


### 2.分类

1. DTD：一种简单的约束技术
2. Schema：一种复杂的约束技术

#### DTD

* 引入dtd文档到xml文档中
    * 内部dtd：将约束规则定义在 xml 文档中
    * 外部dtd：将约束规则定义在外部的dtd文件中
        * 本地：`<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">`
        * 网络：`<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">`



- student.xml
```xml
<?xml version='1.0' encoding='utf-8' ?>
<!-- 外部本地dtd -->
<!DOCTYPE students SYSTEM "student.dtd">
<!--
内部dtd
<!DOCTYPE students [
        <!ELEMENT students (student*) >
        <!ELEMENT student (name,age,sex) >
        <!ELEMENT name (#PCDATA) >
        <!ELEMENT age (#PCDATA) >
        <!ELEMENT sex (#PCDATA) >
        <!ATTLIST student number ID #REQUIRED>
        ]>
-->

<students>
    <student number="s001">
        <name>zhangsan</name>
        <age>20</age>
        <sex>male</sex>
    </student>

    <student number="s002">
        <name>lisi</name>
        <age>21</age>
        <sex>female</sex>
    </student>

</students>
```
- student.dtd

```dtd
<!ELEMENT students (student*) >
<!ELEMENT student (name,age,sex) >
<!ELEMENT name (#PCDATA) >
<!ELEMENT age (#PCDATA) >
<!ELEMENT sex (#PCDATA) >
<!ATTLIST student number ID #REQUIRED>
```



#### Schame

* student.xsd

```xml
<?xml version="1.0"?>
<xsd:schema xmlns="http://www.itcast.cn/xml"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.itcast.cn/xml" elementFormDefault="qualified">

    <xsd:element name="students" type="studentsType" />
        <xsd:complexType name="studentsType">
            <xsd:sequence>
                <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:complexType>

        <xsd:complexType name="studentType">
            <xsd:sequence>
                <xsd:element name="name" type="xsd:string"/>
                <xsd:element name="age" type="ageType"/>
                <xsd:element name="sex" type="sexType"/>
            </xsd:sequence>
            <xsd:attribute name="number" type="numberType" use="required" />
        </xsd:complexType>

        <xsd:simpleType name="sexType">
            <xsd:restriction base="xsd:string">
                <xsd:enumeration value="male"/>
                <xsd:enumeration value="female"/>
            </xsd:restriction>
        </xsd:simpleType>

        <xsd:simpleType name="ageType">
            <xsd:restriction base="xsd:integer">
                <xsd:minInclusive value="0"/>
                <xsd:maxInclusive value="256"/>
            </xsd:restriction>
        </xsd:simpleType>


        <xsd:simpleType name="numberType">
            <xsd:restriction base="xsd:string">
                <xsd:pattern value="heima_\d{4}"/>
            </xsd:restriction>
        </xsd:simpleType>
</xsd:schema>
```



- student.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    1. 填写xml文档的根元素
    2. 引入xsi的前缀。xmlns:xsi="http://www.ws.org/2001/XMLSchema-instance"
    3. 引入xsd文件【命名空间】。
	xsi:schemaLocation="http://itcast.cn/xml【命名空间】 student.xsd【文件路径】"
    4. 为每一个xsd约束声明一个前缀，作为标识 xmlns:别名="http://www.itcast.cn/xml"
-->

<a:students xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:a="http://www.itcast.cn/xml"
          xsi:schemaLocation="http://itcast.cn/xml student.xsd">

    <a:student number="heima_0001">
        <a:name>zhangsan</a:name>
        <a:age>20</a:age>
        <a:sex>male</a:sex>
    </a:student>
</a:students>
```



## 4.解析

- 操作xml文档，将文档中的数据读取到内存中

### 1.操作xml文档
1. 解析(读取)：将文档中的数据读取到内存中
2. 写入：将内存中的数据保存到xml文档中。持久化存储

### 2.解析xml的方式

1. DOM：将标记语言一次性加载进内存，在内存中形成一颗DOM树
    * 优点：操作方便，可以对文档进行CRUD的所有操作
    * 缺点：占内存

2. SAX：逐行读取，基于事件驱动的。
    * 优点：不占内存
    * 缺点：只能读取，不能增删改

> 备注：
>  一般在 服务器端 使用 DOM 解析 xml ，而在内存比较小的 移动端 使用 SAX解析 xml，JavaEE是在服务器端开发的，学习 DOM
> 

### 3.xml常见的解析器

1. JAXP：sun公司提供的解析器，支持dom和sax两种思想
2. DOM4J：一款非常优秀的解析器
3. Jsoup：jsoup 是一款 Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。
4. PULL：Android操作系统内置的解析器，sax方式的。

### 4.Jsoup

#### 快速入门

- 步骤：
    1. 导入 jar 包
    2. 获取 Document对象
    3. 获取对应的标签的 Element 对象
    4. 获取数据

- student.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<students>
    <student number="heima_0001">
        <name>zhangsan</name>
        <age>20</age>
        <sex>male</sex>
    </student>

    <student number="heima_0002">
        <name>lisi</name>
        <age>22</age>
        <sex>female</sex>
    </student>
</students>
```

- JsoupDemo1.java

```java
package com.my.xml.jsoup;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.File;
import java.io.IOException;

/**
 * Jsoup快速入门
 */
public class JsoupDemo1 {
    public static void main(String[] args) throws IOException {
        // 1.导入jar包
        // 2.获取Document对象，根据xml文档获取

        // 2.1 获取xml文档的路径
        String path = JsoupDemo1.class.getClassLoader().getResource("student.xml").getPath();
        // 2.2 解析xml文档，加载文档进内存，获取 dom 树 --> Document
        Document document = Jsoup.parse(new File(path), "utf-8");
        // 3. 获取元素Element

        Elements elements = document.getElementsByTag("name");
//        System.out.println(elements.size());
        // 3.1 获取第一个name的element对象
        Element element = elements.get(0);
        // 3.2 获取数据
        String name = element.text(); // 获取文本数据
        System.out.println(name); // zhangsan
    }
}
```

#### Jsoup对象

* **Jsoup**：工具类，可以解析 html 或 xml 文档，返回 Document对象
    * parse：解析 html 或 xml 文档，返回 Document
        * parse(File in, String charsetName)：解析 xml 或 html 文件的
        * parse(String html)：解析 xml 或 html 字符串
        * parse(URL url, int timeoutMillis)：通过网络路径获取指定的 html 或 xml 的文档对象

- 示例:

```java
package com.my.xml.jsoup;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;

import java.io.IOException;
import java.net.URL;

/**
 * Jsoup对象
 */
public class JsoupDemo2 {
    public static void main(String[] args) throws IOException {
        // 1.parse(File in, String charsetName):解析 xml 或 html文件
/*
        String path = JsoupDemo2.class.getClassLoader().getResource("student.xml").getPath();
        Document document = Jsoup.parse(new File(path), "utf-8");
        System.out.println(document);
*/

        // 2.parse(String html)
/*
        String str = "<?xml version=\"1.0\" encoding=\"UTF-8\" ?>\n" +
                "\n" +
                "<students>\n" +
                "    <student number=\"heima_0001\">\n" +
                "        <name>zhangsan</name>\n" +
                "        <age>20</age>\n" +
                "        <sex>male</sex>\n" +
                "    </student>\n" +
                "\n" +
                "    <student number=\"heima_0002\">\n" +
                "        <name>lisi</name>\n" +
                "        <age>22</age>\n" +
                "        <sex>female</sex>\n" +
                "    </student>\n" +
                "</students>\n";
        Document document = Jsoup.parse(str);
        System.out.println(document);
*/
        // 3.parse(URL url, int timeoutMillis):通过网络路径获取指定的 html
        URL url = new URL("https://baike.baidu.com/item/jsoup");
        Document document = Jsoup.parse(url, 10000);
        System.out.println(document);

    }
}
```

#### Document对象

* **Document**：文档对象。代表内存中的 dom 树
    * 获取Element对象
        * getElementsByTag(String tagName)：根据标签名获取元素对象集合
        * getElementsByAttribute(String key)：根据属性名称获取元素对象集合
        * getElementsByAttributeValue(String key, String value)：根据对应的属性名和属性值获取元素对象集合
        * getElementById(String id)：根据 id 属性值获取唯一的 element 对象

- 示例:

```java
package com.my.xml.jsoup;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.File;
import java.io.IOException;

/**
 * Doucument(Element)对象
 */
public class JsoupDemo3 {
    public static void main(String[] args) throws IOException {
        // 获取student.xml的路径
        String path = JsoupDemo2.class.getClassLoader().getResource("student.xml").getPath();
        // 获取document对象
        Document document = Jsoup.parse(new File(path), "utf-8");
        // 1.获取所有 student 对象们
        Elements elements = document.getElementsByTag("student");
        System.out.println(elements);
        System.out.println("===============");

        // 2.获取属性名为 id 的元素对象们
        Elements elements1 = document.getElementsByAttribute("id");
        System.out.println(elements1);
        System.out.println("===============");

        // 3.获取属性名为number，属性值为heima_0001的元素
        Elements elements2 = document.getElementsByAttributeValue("number", "heima_0001");
        System.out.println(elements2);
        System.out.println("===============");

        // 4.获取id为name的对象
        Element element = document.getElementById("name");
        System.out.println(element);
    }
}
```

#### Elements对象

* **Elements**：元素 Element 对象的集合。可以当作 `ArrayList<Element>` 来使用

#### Element对象

* Element：元素对象
   1. 获取子元素对象
        * getElementsByTag(String tagName)：根据标签名获取元素对象集合
        * getElementsByAttribute(String key)：根据属性名称获取元素对象集合
        * getElementsByAttributeValue(String key, String value)：根据对应的属性名和属性值获取元素对象集合
        * getElementById(String id)：根据 id 属性值获取唯一的 element 对象

    2. 获取属性值
        * String attr(String attributeKey)：根据属性名称获取属性值

    3. 获取文本内容
        * String text()：获取所有子标签的纯文本内容
        * String html()：获取标签体的所有内容(包括子标签标签和文本内容)

#### Node对象

* Node：节点对象
    * 是 Document 和 Element 的父类

#### 快捷查询方式

1. selector：选择器
    * 使用的方法：Elements	select(String cssQuery)
        * 语法：参考 Selector 类中定义的语法
    
1. XPath：XPath即为XML路径语言（XML Path Language），它是一种用来确定XML（标准通用标记语言的子集）文档中某部分位置的语言。
    * 使用 Jsoup 的Xpath 需要额外导入 jar 包
    * 查询 W3cshool 参考手册，使用XPath的语法完成查询
    * 示例：

```java
package com.my.xml.jsoup;

import cn.wanghaomiao.xpath.exception.XpathSyntaxErrorException;
import cn.wanghaomiao.xpath.model.JXDocument;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;

import java.io.File;
import java.io.IOException;

/**
 * XPath查询
 */
public class JsoupDemo6 {
    public static void main(String[] args) throws IOException, XpathSyntaxErrorException {
        // 1.获取student.xml的path
        String path = JsoupDemo6.class.getClassLoader().getResource("student.xml").getPath();
        // 2.获取Document对象
        Document document = Jsoup.parse(new File(path), "utf-8");
        // 3.根据 Document对象，获取 JXDocument对象
        JXDocument jxDocument = new JXDocument(document);

        // 4.结合Xpath语法查询
        // 4.1 查询所有的student标签
        List<JXNode> jxNodes = jxDocument.selN("//student");
        for (JXNode jxNode : jxNodes) {
            System.out.println(jxNode);
        }

        System.out.println("--------------");

        // 4.2 查询所有student标签下的 name 标签
        List<JXNode> jxNodes1 = jxDocument.selN("//student/name");
        for (JXNode jxNode : jxNodes1) {
            System.out.println(jxNode);
        }
        System.out.println("--------------");

        // 4.3 查询所有student标签下带有id属性值的name标签
        List<JXNode> jxNodes2 = jxDocument.selN("//student/name[@id]");
        for (JXNode jxNode : jxNodes2) {
            System.out.println(jxNode);
        }
        System.out.println("--------------");

        // 4.4 查询所有student标签下带有id属性值的name标签，并且id属性值为 itcast
        List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id='itcast']");
        for (JXNode jxNode : jxNodes3) {
            System.out.println(jxNode);
        }
    }
}
```



