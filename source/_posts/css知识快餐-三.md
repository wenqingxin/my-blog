---
title: css知识快餐(三)
date: 2018-03-02 15:43:19
tags:
---

## 一、BFC的基本概念
css布局的基本单位是盒子模型，每种盒子有自己的上下文，比如BFC、IFC、FFC、GFC，这个上下文就是决定盒子内部将要按照什么方式渲染。BFC就是块级格式化上下文，它有自己的一套渲染方式。

## 二、BFC的渲染规则
1. 同一BFC垂直方向margin会发生重叠
2. BFC区域不会与浮动元素box发生重叠
3. BFC是个独立的容器，外面不影响里面的元素，里面的元素也不影响外面
4. 计算BFC高度浮动元素也会参加计算

## 三、创建BFC的方法
1. float值不为none
2. position不为static和relative
3. display是inline-block、table、table-cell、table-caption
4. overflow不为visible
<!-- MORE -->

## 四、使用场景
### 1.去除垂直方向的margin重叠
基本原理就是利用渲染规则中的第一条，同一BFC垂直方向margin会发生重叠，那么新建一个BFC就不会发生重叠了，下面有两个场景。
1. 子元素有margin-top的时候，margin部分并没有撑开父元素。这时候父元素上创建一个BFC就能撑开
```javascript
    <section>
        <style>
            .father {
                background-color: red;
                overflow: hidden; /*创建BFC，使垂直方向的边距不重叠*/
            }
            .father .child {
                background-color: yellow;
                margin-top: 10px;
                height: 100px;
            }
        </style>
        <div class="father">
            <div class="child"></div>
        </div>
    </section>
```
2. 相邻像个兄弟元素，margin-top与margin-bottom会发生重叠，margin的值取较大的一个。解决这个问题的方式是给其中的一个元素加上父元素并创建BFC
```javascript
    <section id="sec1">
        <style>
            .box-top, .box-down {
                height: 100px;
            }
            .box-top {
                background-color: red;
                margin-bottom: 20px;
            }
            .box-down {
                background-color: yellow;
                margin-top: 40px;
            }
            .wapper {
                overflow: hidden; /*创建BFC，使垂直方向的边距不重叠*/
            }
        </style>
        <div class="box-top"></div>
        <div class="wapper">
            <div class="box-down"></div>
        </div>
    </section>
```

### 2.清除浮动
基本原理是第二条，创建BFC的元素不会与浮动元素发生重叠。
```javascript
    <section>
        <style>
            .left, .right {
                height: 100px;
            }
            .left {
                background-color: red;
                float: left;
                width: 100px;
            }
            .right {
                overflow: hidden;
                background-color: yellow;/*创建BFC，不会与块元素发生重叠*/
            }
        </style>
        <div class="left"></div>
        <div class="right"></div>  
    </section>
```