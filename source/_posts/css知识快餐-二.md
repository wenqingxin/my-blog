---
layout: posts
title: css知识快餐(二) 常见布局方式
date: 2018-03-01 22:10:37
tags:
---
## 一、三栏布局的五种实现方式
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0px;
            padding: 0px;
        }

        .layout {
            margin-top: 5px;
            text-align: center;
        }

        .layout .left,.layout .center,.layout .right{
            height: 100px;
        }
        .layout .left{
            background-color: red;
        }
        .layout .center{
            background-color: yellow;
        }
        .layout .right{
            background-color: blue;
        }
        
        .flex {
            margin-top: 110px;
        }
    </style>
</head>
<body>
    <!-- 一、浮动定位三栏布局 -->
    <section>
        <style>
            .float .left {
                float: left;
                width: 300px;
            }
            .float .right {
                float: right;
                width: 300px;
            }
        </style>
        <div class="layout float">
            <div class="left"></div>
            <div class="right"></div>
            <div class="center">        
                <h2>我是浮动三栏布局</h2> 
            </div>
 
        </div>
    </section>

    <!-- 二、绝对定位三栏布局 -->
    <section>
        <style>
            .position .left, .position .center, .position .right {
                position: absolute;
            }
            .position .left {
                width: 300px;
                left: 0px;
            }
            .position .center {
                left: 300px;
                right: 300px;
            }
            .position .right {
                right: 0px;
                width: 300px;
            }
        </style>
        <div class="layout position">    
            <div class="left"></div>
            <div class="center">
                <h2>绝对定位三栏布局</h2>
            </div>
            <div class="right"></div>
        </div>  
    </section>

    <!-- 三、弹性盒子三栏布局 -->
    <section>
        <style>
            .flex {
                display: flex;
            }
            .flex .left {
                width: 300px;
            }
            .flex .center {
                flex: 1
            }
            .flex .right {
                width: 300px;
            }
        </style>
        <div class="layout flex">
            <div class="left"></div>
            <div class="center">
                <h2>我是弹性盒子三栏布局</h2>
            </div>
            <div class="right"></div>
        </div>
    </section>
    
    <!-- 四、table布局 -->
    <section>
        <style>
            .table {
                display: table;
                width: 100%;
            }
            .table .left {
                width: 300px;
                display: table-cell;
            }
            .table .center {
                display: table-cell;
            }
            .table .right {
                width: 300px;
                display: table-cell;
            }
        </style>
        <div class="layout table">
            <div class="left"></div>
            <div class="center">
                <h2>我是表格三栏布局</h2>
            </div>
            <div class="right"></div>    
        </div>
    </section>
    <!-- 五、我是网格三栏布局 -->
    <section>
        <style>
        .grid {
            display: grid;
            grid-template-rows: 100px;
            grid-template-columns: 300px auto 300px;
        }
        </style>
        <div class="layout grid">
            <div class="left"></div>
            <div class="center">
                <h2>我是网格三栏布局</h2>
            </div>
            <div class="right"></div>
        </div>
    </section>
</body>
</html>
```
<!-- MORE -->
## 二、效果
{% asset_img show.png 效果%}
## 三、每种方式的优缺点
1. 浮动三栏布局
优点：兼容性好
缺点：浮动后会产生浮动流，影响周围元素，需要处理
2. 绝对定位三栏布局
优点：快捷，方便
缺点：脱离文档流，后面的元素会看不见，可用性比较差
3. 弹性盒子三栏布局
优点：不会脱离文档流
缺点：pc点兼容比较差
4. 表格三栏布局
优点：兼容性好
缺点：比较繁琐，当一个单元格高度增加，其他同一行的单元格也会增加，有的时候并不需要这样
5. 网格布局
优点：方便、更加灵活的排列内部元素
缺点：当一个单元格高度增加，其他同一行的单元格也会增加
## 四、如果没有高度
源代码里面去掉高度，增加内容让内容自动撑开的情况
{% asset_img chengkai.png 效果%}
由此可见，flex和表格布局是会自动撑开的，效果最理想。