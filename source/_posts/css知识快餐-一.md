---
layout: posts
title: css知识快餐(一) 样式优先级
date: 2018-02-11 20:29:47
tags:
---
## 一、css的优先级类别
通常情况下，我们可以按照一下优先级确定某个元素的样式
1. 样式后面加!important的(权重值为<font color="red">__Infinity__</font>)；
2. 内联样式style添加的样式(权重值为<font color="red">__1000__</font>)；
3. id选择器添加的样式(权重值为<font color="red">__100__</font>)；
4. 类选择器、属性选择器、伪类选择器添加的样式(权重值为<font color="red">__10__</font>)；
5. 标签选择器添加的样式(权重值为<font color="red">__1__</font>);
6. 通配符选择器,>子选择器，+相邻元素选择器添加的样式(权重值为<font color="red">__0__</font>)；
<!-- more -->

## 二、样式类别的权重
在实际开发当中，某个元素的某个样式往往是多种上述类别的样式优先级的组合，那这种情况我们又如何确定样式的优先级呢？一般情况下，就是从左到右<font color="red">__计算样式权重的和，权重值大的优先级就高__</font>(更准确的做法请看第三点)。
{% asset_img css_weight.jpg css样式权重 %}
![img](/wenqingxin/my-blog/blob/master/source/_posts/css知识快餐-一/css_weight.jpg)
举个栗子：
``` css
div.cls-p p.cls-c {
    background-color: yellow;
}
p#ide {
    background-color: red;
}
```
``` html
    <div class="cls-p">
        <p id="ide" class="cls-c">我是什么颜色</p>
    </div>
```
其中，第一个选择器的样式权重值和为 1 + 10 + 1 + 10 = 22；
第二个选择器的样式权重值为 1 + 100 = 101；
第二个选择器权重值(101)大于第一个选择器权重值(22)，所以颜色就应该是红色。
## 三、需要注意的点
跨权重级别的权重值累加方式
    如果同一个元素被两个选择器添加样式是这样：
``` css
.class1.class2.class3.class4.class5.class6.class7.class8.class9.class10.class11 {
    background-color: yellow;
}
#id {
    background-color: red;
}
```
按上面的计算原则，10 \* 11 > 100,我们会得出颜色是yellow的结论，但事实并非这样，实际结果仍然是red。为什么呢？原因就在于：
在旧版本的火狐、IE浏览器中<font color="red">__权重值的进制并不是十进制__</font>，每一个权重位是由8位来表示，也就是2^8 256进制，那么如果需要往高位进位的话，需要等于256，如果要让类选择器大于id，需要256个10才等于100，也就是说你需要写超过256个class的权重才能超过id。
在15年后的浏览器不存在低权重位累加后可以大于高权重位的情况了，也就是说<font color="red">__权重位高的选择器的权重永远大于权重位低的选择器__</font>(一个id优先级永远大于n个class)，所以我们在累加权重的时候，需要首先找出权重位最高的选择器累加后先比较，如果能比较出大小，就不用再比较权重位低的了，如果权重位最高的权重值相等，再去比较下一权重位，依此类推。