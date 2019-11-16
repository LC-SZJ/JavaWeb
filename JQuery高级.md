[TOC]

# JQuery高级

## 1. 动画

>   *   三种方式显示和隐藏元素

### 1.默认显示和隐藏方式



| 方法名称                      | 作用                                         |
| ----------------------------- | -------------------------------------------- |
| show([speed,[easing],[fn]])   | 显示隐藏的元素                               |
| hide([speed,[easing],[fn]])   | 隐藏显示的元素                               |
| toggle([speed],[easing],[fn]) | 可见的，切换为隐藏的；隐藏的，切换为可见的。 |

>   *   参数：
>       *   `speed`：动画的速度，三种预定速度之一的字符串("slow","normal", or "fast")或表示动画时长的毫秒数值(如：1000)
>       *   `easing`：用来指定切换效果，默认是"swing"，可用参数"linear"
>           *   `swing`：动画执行时效果是先慢，中间快，最后又慢
>           *   `linear`：动画执行时速度是匀速的
>       *   `fn`：在动画完成时执行的函数，每个元素执行一次。



*   使用：

```javascript
   function showFn() {
       $("#showDiv").show("slow", "swing", function () {
           alert("显示了...")
       });
   }

   function hideFn() {
       $("#showDiv").hide("slow", "swing", function () {
           alert("隐藏了...");
       });
   }

   function toggleFn() {
       $("#showDiv").toggle("slow", "swing", function () {
           alert('toggle...');
       });
   }
```



### 2.滑动显示和隐藏方式



| 方法名称                           | 作用                       |
| ---------------------------------- | -------------------------- |
| slideDown([speed],[easing],[fn])   | 向下滑动来显示隐藏的元素   |
| slideUp([speed,[easing],[fn]])     | 向上滑动来隐藏的元素       |
| slideToggle([speed],[easing],[fn]) | 显示改为隐藏，隐藏改为显示 |



*   使用：

```javascript
function showFn() {
     // 向下滑动
     $("#showDiv").slideDown("slow", "swing");
 }

 function hideFn() {
     // 向上滑动
     $("#showDiv").slideUp("slow", "swing");
 }

 function toggleFn() {
     // 显示改为隐藏，隐藏改为显示
     $("#showDiv").slideToggle("slow");
 }
```



### 3.淡入淡出显示和隐藏方式



| 方法名称                          | 作用                       |
| --------------------------------- | -------------------------- |
| fadeIn([speed],[easing],[fn])     | 淡入显示元素               |
| fadeOut([speed],[easing],[fn])    | 淡出隐藏元素               |
| fadeToggle([speed,[easing],[fn]]) | 显示改为隐藏，隐藏改为显示 |



*   使用：

```javascript
 function showFn() {
       $("#showDiv").fadeIn("slow");

   }
   function hideFn() {
       $("#showDiv").fadeOut("slow");
   }
   function toggleFn() {
       $("#showDiv").fadeToggle("slow");
   }
```



## 2. 遍历

### 1.js的遍历方式：

*   语法：`for(初始化值;循环结束条件;步长)`

*   使用

```javascript
$(function () {
     // js方式遍历
     // 获取所有ul下的li
     var citys = $("#city li");
     // 遍历li
     for (var i = 0; i < citys.length; i++) {
         if ("上海" == citys[i].innerText) {
             // break;
             continue;
         }
         // 获取内容
         alert(i + ":" + citys[i].innerText);
     }
 });
```

### 2. jquery的遍历方式



#### 1.jq对象.each(callback)

1.  语法：

    1.  `jquery对象.each(function(index,element){});`
    2.  `jquery对象.each(function(){});`
        * `index`：就是元素在集合中的索引
        * `element`：就是集合中的每一个元素对象 
        * `this`：集合中的每一个元素对象
    
2. callback（回调函数)的返回值：
    * `true`：如果当前function返回为false，则结束循环(break)。
    * `false`：如果当前function返回为true，则结束本次循环，继续下次循环(continue)

    

*   使用：

```javascript
$(function () {
   // jquery方式遍历 ul 下的 li
   var citys = $("#city li");
   citys.each(function (index, element) {
   // 获取li对象的第一种方式:this
   // alert(this.innerText);
   // alert($(this).html());
   // 获取li对象的第二种方式:在回调函数中定义参数 index(索引) element(元素对象)
   // alert(index + ":" + element.innerText);
   // 判断如果是上海，则结束循环
  if ("上海" == $(element).html()) {
      // 如果当前function返回为false，则结束本次循环(break)
      // 如果返回为true，则结束本次循环，继续下次循环(continue)
      return false;
     }
  alert(index + ":" + $(element).html());
    });
});
```



#### 2.$.each(object, [callback])

*   使用

```javascript
// jquery方式遍历 ul 下的 li
 var citys = $("#city li");
jQuery.each(citys, function () {
    alert($(this).html());
});
// $代表jQuery
// $.each(object, [callback])
 $.each(citys, function () {
     // this代表每一个js对象
     alert($(this).html());
 })
```

>   *   citys就是一个jquery对象，citys[索引]：代表jquery对象转换为js对象



### 3.for ... of

*   使用

```javascript
// jquery方式遍历 ul 下的 li
 var citys = $("#city li");
// for ... of：Jquery 3.0 版本之后提供的方式
for (li of citys) {
     alert($(li).html());
 }
```

## 3. 事件绑定

### 1. jquery标准的绑定方式

*   语法：`jq对象.事件方法(回调函数);`

>   ==注==：如果调用事件方法，不传递回调函数，则会触发浏览器默认行为。
>
>   `表单对象.submit();` // 让表单提交

*   使用

```html
<input id="name" type="text" value="绑定点击事件">
```

```javascript
$("#name").click(function () {
     alert("我被点击了...");
 });
 $("#name").mouseover(function () {
     alert("鼠标来了...");
 });

 $("#name").mouseout(function () {
     alert("鼠标走了...");
 });
 // 简化书写，链式编程
$("#name").mouseover(function () {
     alert("链式1...");
 }).mouseout(function () {
     alert("链式2...");
 });

 alert("我要获得焦点了...");
// 没有写回调函数，默认会让文本输入框获得焦点
 $("#name").focus(); 

$("#name").blur(function(){
    alert("我要失去焦点了...");
});
```



### 2.on绑定事件/off解除绑定

*   语法：

    *   `jq对象.on("事件名称", 回调函数)`
    *   `jq对象.off("事件名称")`

    >   如果`off 方法`不传递任何参数，则将组件上的所有事件全部解绑。

*   使用：

```html
<input id="btn" type="button" value="使用on绑定点击事件">
<input id="btn2" type="button" value="使用off解绑点击事件">
```

```javascript
  // 1.使用on给按钮绑定单击事件 click
  $("#btn").on("click", function () {
      alert("我被点击了...");
  });

  // 2.使用off给btn解除单击事件
  $("#btn2").click(function () {
      $("#btn").off("click");
      // 将组件上的所有事件全部解绑
      // $("#btn").off("");
  });
```



### 3.事件切换：`toggle`

*   语法：`jq对象.toggle(fn1, fn2, ...)`
    *    当单击jq对象对应的组件后，会执行fn1.第二次点击会执行fn2.....

* 注意：**1.9版本 .toggle() 方法被删除,jQuery Migrate（迁移）插件可以恢复此功能。**
			 
			 `<script src="js/jquery-migrate-1.0.0.js" type="text/javascript" charset="utf-8">`

*   使用：

```html
<input id="btn" type="button" value="事件切换">
<div id="myDiv" style="width:300px;height:300px;background:pink">
    点击按钮变成绿色，再次点击红色
</div>
```

```javascript
  $(function () {
      // 获取按钮，调用toggle方法
      $("#btn").toggle(function () {
          // 改变div 的背景色 backgroundColor 为 green
          $("#myDiv").css("backgroundColor", "green");
      }, function () {
          // 改变div 的背景色 backgroundColor 为 pink
          $("#myDiv").css("backgroundColor", "red");
      });
  });
```



## 4.案例

### 1. 广告的自动显示与隐藏

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>广告的自动显示与隐藏</title>
    <style>
        #content {
            width: 100%;
            height: 500px;
            background: #999
        }
    </style>
    <!--引入jquery-->
    <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>
    <script>
        /*
            需求：
                1. 当页面加载完，3秒后。自动显示广告。
                2. 广告显示5秒后，自动消失。
            分析：
                1. 使用定时器来完成，setTimeout(执行一次定时器)
                2. JQuery的显示和隐藏动画效果其实就是控制 display属性
                3. 使用 show/hide方法来完成广告的显示和隐藏
         */
        // 入口函数，在页面加载完成之后，定义计时器，调用这两个方法
        $(function () {
            // 定义计时器，显示广告, 3秒执行一次
            setTimeout(adShow, 3000);
            // 定义计时器，隐藏广告, 8秒执行一次
            setTimeout(adHide, 8000);
        });

        // 显示广告
        function adShow() {
            $("#ad").slideDown("slow");
        }

        // 隐藏广告
        function adHide() {
            $("#ad").slideUp("slow");
        }
    </script>
</head>
<body>
<!-- 整体的DIV -->
<div>
    <!-- 广告DIV -->
    <div id="ad" style="display: none;">
        <img style="width:100%" src="../img/adv.jpg"/>
    </div>

    <!-- 下方正文部分 -->
    <div id="content">
        正文部分
    </div>
</div>
</body>
</html>
```



### 2.抽奖案例

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>jquery案例之抽奖</title>
    <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>
    <script>
        /*
             分析：
                 1. 给开始按钮绑定单击事件
                     1.1 定义循环定时器
                     1.2 切换小相框的src属性
                         * 定义数组，存放图片资源路径
                         * 生成随机数。数组索引

                 2. 给结束按钮绑定单击事件
                     1.1 停止定时器
                     1.2 给大相框设置src属性
          */
        // 用一维数组来存放抽奖图片的路径
        var imgs = [
            "../img/man00.jpg",
            "../img/man01.jpg",
            "../img/man02.jpg",
            "../img/man03.jpg",
            "../img/man04.jpg",
            "../img/man05.jpg",
            "../img/man06.jpg"
        ];
        // 设置开始按钮和停止按钮是否禁用的状态
        function setDisabled(start, stop) {
            $("#startID").prop("disabled", start);
            $("#stopID").prop("disabled", stop);
        }

        var startId; // 开始定时器的id
        var index; // 随机索引
        $(function () {
            // 处理按钮是否可以使用的效果
            // setDisabled(false, true);

            // 初始状态，开始按钮 不禁用，停止按钮 禁用
            /*$("#startID").prop("disabled", false);
            $("#stopID").prop("disabled", true);*/
            setDisabled(false, true);

            // 1.给开始按钮绑定单击事件
            $("#startID").click(function () {

                // 处理按钮是否可以使用的效果
                /*$("#startID").prop("disabled", true);
                $("#stopID").prop("disabled", false);*/
                setDisabled(true, false);

                // 1.1 定义循环计时器，20毫秒执行一次
                startId = setInterval(function () {
                    // 1.2 生成随机角标 0-6
                    // 0.000-0.999 --> * 7 --> 0.000-0.6999 --> 向下取整 0-6
                    index = Math.floor(Math.random() * 7);
                    // 1.3 设置小相框的src属性
                    $("#img1ID").prop("src", imgs[index]);
                }, 20);
            });

            // 2. 给停止按钮绑定单击事件
            $("#stopID").click(function () {

                /*$("#startID").prop("disabled", false);
                $("#stopID").prop("disabled", true);*/
                setDisabled(false, true);

                // 1.1 停止计时器
                clearInterval(startId);
                // 1.2 给大相框设置src
                $("#img2ID").prop("src", imgs[index]).hide();
                // 1秒之后显示
                $("#img2ID").show(1000);
            });
        });

    </script>
</head>
<body>

<!-- 小像框 -->
<div style="border-style:dotted;width:160px;height:100px">
    <img id="img1ID" src="../img/man00.jpg" style="width:160px;height:100px"/>
</div>

<!-- 大像框 -->
<div
        style="border-style:double;width:800px;height:500px;position:absolute;left:500px;top:10px">
    <img id="img2ID" src="../img/man00.jpg" width="800px" height="500px"/>
</div>

<!-- 开始按钮 -->
<input
        id="startID"
        type="button"
        value="点击开始"
        style="width:150px;height:150px;font-size:22px"
        onclick="imgStart()">

<!-- 停止按钮 -->
<input
        id="stopID"
        type="button"
        value="点击停止"
        style="width:150px;height:150px;font-size:22px"
        onclick="imgStop()">
</body>
</html>
```



## 5.插件

>   增强JQuery的功能

### 1.实现方式：

| 使用方式               | 作用                                      |
| ---------------------- | ----------------------------------------- |
| `$.fn.extends(object)` | 增强通过Jquery获取的对象的功能 `$("#id")` |
| `$.extends(object)`    | 增强JQuery对象自身的功能  `$/jQuery`      |



*   `$.fn.extends(object)` 使用：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>01-jQuery对象进行方法扩展</title>
    <script src="../js/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>
    <script type="text/javascript">
        /*
        使用jquery插件 给jq对象添加2个方法
        check()选中所有的复选框，uncheck()取消选中所有的复选框
        */
        // 定义jquery的对象插件
        $.fn.extend({
            check: function () {
                $(this).prop("checked", true);
            },
            uncheck: function () {
                $(this).prop("checked", false);
            }
        });

        // 点击全选
        function checkFn() {
            // 获取所有的复选框
            $("input[type='checkbox']").check();
        }

        // 点击取消全部选
        function uncheckFn() {
            $("input[type='checkbox']").uncheck();
        }

       /* $(function () {
            $("#btn-check").click(function () {
                $("input[type='checkbox']").check();
            });

            $("#btn-uncheck").click(function () {
                $("input[type='checkbox']").uncheck();
            });
        });*/
    </script>
</head>
<body>
<input id="btn-check" type="button" value="点击选中复选框" onclick="checkFn()">
<input id="btn-uncheck" type="button" value="点击取消复选框选中" onclick="uncheckFn()">
<br/>
<input type="checkbox" value="football">足球
<input type="checkbox" value="basketball">篮球
<input type="checkbox" value="volleyball">排球
</body>
</html>
```



*   `$.extends(object)` 使用：

```javascript
//对全局方法扩展2个方法，扩展min方法：求2个值的最小值；扩展max方法：求2个值最大值
$.extend({
    // 返回两数的最大值
    max: function (a, b) {
        return a >= b ? a : b;
    },
    // 返回两数的最小值
    min: function (a, b) {
        return a <= b ? a : b;
    }
});

$(function () {
    max = $.max(3, 5);
    alert(max);
    min = $.min(1, 3);
    alert(min);
});
```

















