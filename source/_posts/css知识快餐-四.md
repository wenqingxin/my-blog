---
title: css知识快餐-四 层叠上下文
date: 2018-04-16 14:00:29
tags:
---
## 一、什么是层叠上下文
层叠上下文(stacking context)是html页面中的一个三维的概念，除了屏幕平面的x、y轴，在我们面向屏幕的方向上还有一个Z轴，页面元素在Z抽上的离我们最近的就会覆盖离我们远的元素，元素在Z轴上的远近也就是我们说的层叠顺序。
<!-- MORE -->
## 二、什么是层叠顺序
层叠顺序(stacking order)决定一个层叠上下文中元素在Z抽上的顺序。规则如下
![层叠顺序](http://image.zhangxinxu.com/image/blog/201601/2016-01-09_211116.png "层叠顺序")
{% asset_img stack-order.png 层叠顺序 %}

## 三、层叠上下文的特点
1. 层叠上下文的层叠水平要比普通元素高；
2. 层叠上下文可以阻断元素的混合模式（[mix-blend-mode](http://www.zhangxinxu.com/wordpress/2016/01/understand-css3-isolation-isolate/)）；
3. 层叠上下文可以嵌套，层叠顺序只会在该层叠上下文起作用而不会影响外部。

## 四、z-index与positionde关系
z-index起作用的前提是设置了position 为relative、absolute、fixed中的一种。

## 五、层叠上下文的创建
1. 根层叠上下文html
2. 对于包含有position:relative/position:absolute的定位元素，以及FireFox/IE浏览器（不包括Chrome等webkit内核浏览器）（目前，也就是2016年初是这样）下含有position:fixed声明的定位元素，当其z-index值不是auto的时候，会创建层叠上下文。
1. z-index值不为auto的flex项(父元素display:flex|inline-flex).
2. 元素的opacity值不是1.
3. 元素的transform值不是none.
4. 元素mix-blend-mode值不是normal.
5. 元素的filter值不是none.
6. 元素的isolation值是isolate.
7. will-change指定的属性值为上面任意一个。
8. 元素的-webkit-overflow-scrolling设为touch.

## 六、层叠上下文与层叠顺序
层叠顺序的比较，要在同一个层叠上下文下才有意义，它们遵守以下原则：
谁大谁上：当具有明显的层叠水平标示的时候，如识别的z-indx值，在同一个层叠上下文领域，层叠水平值大的那一个覆盖小的那一个。
后来居上：当元素的层叠水平一致、层叠顺序相同的时候，在DOM流中处于后面的元素会覆盖前面的元素。

参考资料：[深入理解CSS中的层叠上下文和层叠顺序](http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)