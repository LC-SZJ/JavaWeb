[TOC]

# Bootstrap


## 1.概述

- **概念**：**一个前端开发的框架**，**Bootstrap**是美国**Twitter公司**的设计师Mark Otto和Jacob Thornton合作**基于HTML、CSS、JavaScript** 开发的简洁、直观、强悍的**前端开发框架**，使得 Web 开发更加快捷。
    - **框架：一个半成品软件**，开发人员可以在框架的基础上，再进行开发，简化编码。
    - **好处**：
        1. 定义了很多 css 式和 js 插件。我们开发人员可以直接使用这些样式和插件得到丰富的页面效果。
        2. **响应式布局**：
            - **同一套页面可以兼容不同分辨率的设备。**

## 2.快速入门

1. 下载 Bootstrap
2. 在项目中将这三个文件夹复制
3. 创建html页面，引入必要的资源文件

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

</head>
<body>
<h1>你好，世界！</h1>

<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

## 3.响应式布局

* 同一套页面可以兼容不同分辨率的设备
* 实现：依赖于**栅格系统**：将一行平均分成12个格子，可以指定元素占几个格子。
* 步骤：
    1. **定义容器**。相当于之前的 table
        * 容器分类：
            1. **container：两边留白**
            2. **container-fluid：每一种设备都是 100% 宽度**
    2. **定义行**。相当于之前的 tr	样式：**row**
    3. **定义元素**。**指定该元素在不同的设备上，所占的格子数目**。 **样式**：**col-设备代号-格子数目**
        * **设备代码**：
            1. **xs**：超小屏幕 手机 (<768px)
            2. **sm**：小屏幕 平板 (≥768px)
            3. **md**：中等屏幕 桌面显示器 (≥992px)
            4. **lg**：大屏幕 大桌面显示器 (≥1200px)


> 注意：
> 1. 一行中如果格子的数目超过 12，则超出部分会自动换行。
> 2. 栅格类属性可以【向上兼容】。栅格类适用于与屏幕宽度大于或等于分界点大小的设备。
> 3. 如果【真实设备】宽度小于了设置栅格类属性的【设备代码的最小值】，会一个元素占满一整行。

- **示例**：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 栅格</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <style>
        .inner {
            border: 1px solid red;
        }
    </style>

</head>
<body>
    <!--1.定义容器-->
    <div class="container">
        <!--2.定义行-->
        <div class="row">
            <!--3.定义元素
                在大显示器上一行12个格子
                在pad上一行6个格子
            -->
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
            <div class="col-lg-1 col-md-2 inner">栅格</div>
        </div>

    </div>

<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

## 4.CSS样式和JS插件

### 1.全局CSS样式

* 按钮：`class="btn btn-default"`
* 图片：
    * `class="img-responsive"`：图片在任意尺寸都占100%
    * 图片形状
        * `<img src="..." alt="..." class="img-rounded">`
        * `<img src="..." alt="..." class="img-circle">`
        * `<img src="..." alt="..." class="img-thumbnail">`

* 表格
    * `table`
    * `table-bordered`
    * `table-hover`
* 表单
    * 给表单项添加：`class="form-control"`

### 2.组件

* 导航条
* 分页条

### 3.插件
* 轮播图

## 5.案例:黑马旅游网

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 黑马旅游网</title>

    <!-- Bootstrap -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <style>
        .paddtop {
            padding-top: 10px;
        }

        /*搜素按钮*/
        .search-btn {
            float: left;
            border: 1px solid #ffc900;
            width: 90px;
            height: 35px;
            background-color: #ffc900;
            text-align: center;
            line-height: 35px;
            margin-top: 15px;
        }

        /*搜索框*/
        .serach-input {
            float: left;
            border: 2px solid #ffc900;
            width: 400px;
            height: 35px;
            padding-left: 8px;
            margin-top: 15px;
        }

        /*小图标*/
        .img_icon {
            width: 50px;
            height: 50px;
        }

        /*精选*/
        .selected {
            border-bottom: 2px solid #ffc900;
            padding: 5px;
        }

        /*头像*/
        .img_touxiang {
            width: 400px;
            height: 550px;
        }

        /*公司简介*/
        .company {
            background-color: #ffc900;
            height: 40px;
            text-align: center;
            line-height: 40px;
            font-size: 8px;
        }
    </style>

</head>
<body>
<!--1.页眉部分-->
<header class="container-fluid">
    <div class="row">
        <img src="img/top_banner.jpg" class="img-responsive">
    </div>

    <div class="row paddtop">
        <div class="col-md-3">
            <img src="img/logo.jpg" class="img-responsive">
        </div>
        <div class="col-md-5">
            <input type="text" placeholder="请输入线路" class="serach-input">
            <a href="#" class="search-btn">搜索</a>
        </div>
        <div class="col-md-4">
            <img src="img/logo.jpg" class="img-responsive">
        </div>
    </div>

    <!--导航条-->
    <div class="row">
        <nav class="navbar navbar-default">
            <div class="container-fluid">
                <!-- Brand and toggle get grouped for better mobile display -->
                <div class="navbar-header">
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                            data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                        <span class="sr-only">Toggle navigation</span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                    </button>
                    <a class="navbar-brand" href="#">首页</a>
                </div>

                <!-- Collect the nav links, forms, and other content for toggling -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                    <ul class="nav navbar-nav">
                        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
                        <li><a href="#">Link</a></li>
                        <li><a href="#">Link</a></li>
                        <li><a href="#">Link</a></li>
                        <li><a href="#">Link</a></li>
                        <li><a href="#">Link</a></li>
                        <li><a href="#">Link</a></li>
                    </ul>
                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
    </div>
    <!--轮播图-->
    <div class="row">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
            <!-- Indicators -->
            <ol class="carousel-indicators">
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- Wrapper for slides -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <img src="img/banner2.jpg" alt="...">
                    <div class="carousel-caption">
                    </div>
                </div>
                <div class="item">
                    <img src="img/banner3.jpg" alt="...">
                    <div class="carousel-caption">
                    </div>
                </div>
                <div class="item">
                    <img src="img/banner4.jpg" alt="...">
                    <div class="carousel-caption">
                    </div>
                </div>
            </div>

            <!-- Controls -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                <span class="sr-only">Previous</span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                <span class="sr-only">Next</span>
            </a>
        </div>
    </div>
</header>
<!--2.主体部分-->
<div class="container">
    <div class="row selected">
        <img src="img/icon_1.jpg" class="img_icon">
        <span>景点精选</span>
    </div>

    <div class="row paddtop">
        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/selected_1.jpg">
                <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                <font color="red">￥ 699</font>
            </div>
        </div>

        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/selected_2.jpg">
                <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                <font color="red">￥ 699</font>
            </div>
        </div>

        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/selected_3.jpg">
                <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                <font color="red">￥ 699</font>
            </div>
        </div>

        <div class="col-md-3">
            <div class="thumbnail">
                <img src="img/selected_4.jpg">
                <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                <font color="red">￥ 699</font>
            </div>
        </div>

    </div>

    <div class="row selected">
        <img src="img/icon_1.jpg" class="img_icon">
        <span>国内游</span>
    </div>

    <div class="row paddtop">
        <div class="col-md-4">
            <img src="img/touxiang1.jpeg" class="img_touxiang">
        </div>

        <div class="col-md-8">
            <div class="row">
                <div class="col-md-4">
                    <div class="thumbnail">
                        <img src="img/selected_1.jpg" class="img-responsive">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <font color="red">￥ 699</font>
                    </div>
                </div>

                <div class="col-md-4">
                    <div class="thumbnail">
                        <img src="img/selected_2.jpg" class="img-responsive">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <font color="red">￥ 699</font>
                    </div>
                </div>

                <div class="col-md-4">
                    <div class="thumbnail">
                        <img src="img/selected_3.jpg" class="img-responsive">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <font color="red">￥ 699</font>
                    </div>
                </div>
            </div>

            <div class="row">
                <div class="col-md-4">
                    <div class="thumbnail">
                        <img src="img/selected_3.jpg" class="img-responsive">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <font color="red">￥ 699</font>
                    </div>
                </div>

                <div class="col-md-4">
                    <div class="thumbnail">
                        <img src="img/selected_2.jpg" class="img-responsive">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <font color="red">￥ 699</font>
                    </div>
                </div>

                <div class="col-md-4">
                    <div class="thumbnail">
                        <img src="img/selected_1.jpg" class="img-responsive">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <font color="red">￥ 699</font>
                    </div>
                </div>
            </div>

        </div>

    </div>

</div>

<!--3.页脚部分-->
<div class="container-fluid">
    <div class="row">
        <img src="img/top_banner2.jpg" class="img-responsive">
    </div>

    <div class="row company">
        江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2019, All Rights Reserved 技术支持 360网站安全检测平台 苏ICP备16007882 苏公网安备
        32132202000574号
    </div>
</div>

<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.2.1.min.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.min.js"></script>
</body>
</html>
```

