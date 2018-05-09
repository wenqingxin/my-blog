---
title: css知识快餐-五 line-height
date: 2018-04-18 12:32:22
tags:
---
## 一、line-height的定义
行高，两行文字基线之间的距离
1. 什么是基线
{% asset_img jixian.png 基线 %}
2. 为什么是基线
其他线比如顶线、底线都是基于基线定义的

## 二、line-height与行内框模型
行内框模型
``` html
<p>一行文字，<em>em</em>标签</p>
```
1. 内容区域 拖动文字选中 一行文字，
2. 内联盒子 em
3. 行框盒子 p标签内部的一行
4. 包含盒子 p标签内部包含的盒子

## 三、line-height高度原理
内联元素高度是由line-height决定的而不是字体。行高满足下面的公式
内容区域高度 + 行间距 = 行高
内容区域高度不一定等于字体大小，只有在simsum字体下内容区域高度等于字体大小

## 四 行高值的含义
 1.5 当前元素内font-size
 150% 父元素font-size

## 五、会继承的css属性
font-size
line-height

## 六、vertical-align
### 具体值
1. 线类
top、middle、baseline、bottom
2. 文本类
text-top、text-bottom
3. 上标下标类
sub、super
4. 数值百分比
20px（相对于baseline的偏移）、20%（相对与行高的数值）、2em

### vertical-align起作用的前提
作用的元素display必须是inline、inline-block、table-cell中的一种。
table-cell的vertical-align只作用自身，在子元素里没有效果。

### inline-block如何确定base-line
inline-block的元素是空的，基线就是底边缘，否则就是受里面的文字或者图片决定的。
{% asset_img inline-block-baseline.png 对齐方式 %}

### vertical-align线类对齐原则
top: 元素的顶部与行的顶部对齐
bottom: 元素的顶部与行的底部部对齐
middle:{% asset_img ver-mid.png 对齐原则 %}
要实现完全居中对齐，设置font-size为0

## 七、实际应用
1. 单行文本居中 
2. 多行文本居中
``` html
    <style>
    .parent {
        line-height: 100px;
        vertical-align: middle;
        text-align: center;
    }
    .child {
        display: inline-block;
        line-height: normal;/*重置父元素继承的行高*/
    }
    </style>
    <div class="parent">
    	<div class="child">
    		<p>1</p>
    		<p>2</p>
    		<p>3</p>
    	</div>
    </div>
```
3. 消除图片下边缘的间隙
图片本身是inline-block元素，所以会受到默认的line-height与vertical-align的作用，由于图片默认是底部与行框盒子的baseline对齐，因此有一部分间隙。消除间隙的方式有三种
1)设置元素display为block，block元素没有vertical-align属性了。
2）设置vertical-align为bottom、middle、top中的一种
3）设置line-height等于0或者font-size（line-height为数字的情况下）等于0
[参考资料](http://www.zhangxinxu.com/wordpress/2009/11/css%E8%A1%8C%E9%AB%98line-height%E7%9A%84%E4%B8%80%E4%BA%9B%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%8F%8A%E5%BA%94%E7%94%A8/)