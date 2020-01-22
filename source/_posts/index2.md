---
title: CSS知识点
date: 2019-12-06 10:36:05
tags: "CSS"
---

CSS知识点
=========


1.基础知识 
----------

-   class应用于概念上相似的元素，而id应用于不同的唯一元素。

-   只在没有现有元素能实现区域分割的情况下使用div。

2.为样式找到应用目标
--------------------

-   **常用的选择器**：元素选择器`div{}`，后代选择器`div p{}`，id选择器`#header{}`，类选择器`.header{}`，建议混合使用

-   **通用选择器**：`*`

-   **高级选择器**:子选择器`ul>li{}`，相邻同胞选择器，根据属性或属性值的属性选择器`input[type="text"]`

3.可视化模型 
------------

> 最重要的3个CSS概念是**浮动，定位和盒模型**

### 3.1 CSS标准盒子模型 

css盒子模型包含了元素内容(content)，内边距(padding)，边框(border)，外边距(margin)几个要素，如下图：
![css-box](https://github.com/hhhwwq/MarkdownPhotos/blob/master/css_box.jpg?raw=true)

通常我们设置背景时就是内容，内边距，边框三部分，如果border设置颜色的时候会显示border颜色，当border颜色是透明时会显示background-color的颜色。外边距是透明的，不会遮挡其他元素。width就是指content的宽度，height就是指content的高度。

-   只有普通文档流中边框的垂直外边距才会发生外边距叠加。行内框，浮动框或绝对定位框之间的外边距不会叠加。

### 3.2 定位

-   使用相对定位时，无论是否移动，元素仍然占据原来的空间，因此，移动元素会使其覆盖其他框。

-   绝对定位：其元素的位置是相对于距离它最近的那个已定位的祖先元素确定的，如果没有已定位的祖先元素，那么其位置是相对于初始包含块的。

-   浮动：脱离普通文档流。clear属性。

4.画三角形
----------

    .sanjiao {
            border-top: 50px solid transparent;
            border-left: 50px solid #f00;
            border-bottom: 50px solid transparent;
            border-right: 0px solid #00f;
            width: 0px;
            height: 0px;
            margin: 50px auto;
        }

梯形由三角形加width构成

5.画对话框 
----------

利用伪元素`::before`将小三角放置在长方形框前面构成对话框

    .dialog::before {
        content: '';
        border-left: 0px solid #6A6;
        border-top: 10px solid transparent;
        border-bottom: 10px solid transparent;
        border-right: 10px solid #6A6;
        position: absolute;
        left: -10px;
        top: 14px;
    }

6.盒模型有哪些？Bootstrap里面用的什么盒模型？
---------------------------------------------

盒模型分为IE盒模型和W3C盒模型。

1.  **W3C标准盒模型content-box**：属性width，height只包含内容content，不包含border和padding。

2.  **IE盒模型border-box**：属性width，height包含border和padding，指的是content+padding+border。
    Bootstrap里面用的是IE盒模型border-box。

7.CSS3有哪些新特性？
--------------------

1.  实现圆角`border-radius`，阴影`box-shadow`，`border-image`;
2.  对文字加特效`text-shadow`，线性渐变`gradient`，旋转`transform`;
3.  `transform`:旋转`rotate`，缩放`scale`，定位`translate`，倾斜`skew`;
4.  增加了更多的`css`选择器，多背景，rgba;
5.  在`css3`中引入了一个伪类`::selection`;
6.  媒体查询，多栏布局

8.为什么要清除浮动？怎么清除浮动？
----------------------------------

**产生原因**：子盒子浮动导致的父盒子内高度为0，父级盒子不能被撑开，发生高度塌陷的情况。

**副作用：**

1.  背景不能显示
2.  边框不能撑开
3.  `margin和padding`值不能正确显示

**清楚浮动的方法：**

1.  给父盒子设置合适的高度；
2.  给父盒子添加样式`overflow:hidden/auto;`(这个属性相当于触发了BFC，让父级紧贴内容，包括使用了浮动的盒子)(为了去除兼容性问题，会添加`zoom:1`);

3.  在父盒子里面的子盒子后面添加一个子盒子，如div，添加样式
    `.clear{ clear:both; }；`

4.  采用伪元素，给父元素追加:after，给父元素添加一个类`.clearfix{content:"";clear:both;}`

9.怎么让文本不自动换行？怎么让超过文本部分变成省略号？ 
------------------------------------------------------

    white-space:nowrap;
    overflow:hidden;
    text-overflow:ellipsis;

10.css中的动画特性可以用js实现，那为什么还要用css来实现？
---------------------------------------------------------

-   当为UI元素采用较小的独立状态时，使用CSS。CSS变换和动画非常适用于从侧面引入导航菜单，或显示工具提示。最后，可以使用JavaScript来控制状态，但动画本身是采用CSS。**用CSS制作动画是让元素在屏幕上移动的最简单方法**。

-   **在需要对动画进行大量控制时，使用JavaScript**。在需要停止，暂停，减速或倒退时，JavaScript也非常有用。

**所以在两者中如何抉择？**

-   渲染线程分为主线程和合成线程，如果CSS动画只是改变transform和opacity，这时整个CSS动画得以在合成线程完成（而JS动画则会在主线程执行，然后触发合成线程进行下一步操作）。在JS执行一些昂贵的任务时，主线程繁忙，CSS动画由于使用了合成线程可以保持流畅。

-   对于低版本浏览器，CSS3可以做到自然降级，而JS则需要撰写额外代码。

-   CSS动画有**天然事件支持，JS则需要自己写事件**。

-   如果有任何动画触发绘画，布局或两者，则需要 “主线程”
    才能完成工作。这对于基于 CSS 和 JavaScript 的动画都是如此。

-   CSS3有兼容性问题，而JS大多时候没有兼容性问题。

11.边距重叠解决方案(BFC) 
------------------------

首先要明确BFC是什么意思，其全英文拼写为 Block Formatting Context
直译为“块级格式化上下文”

**BFC块级格式化上下文的原理**:

1.  内部的Box会在**垂直方向**，从顶部开始**一个接一个**地放置。

2.  Box垂直方向的距离由margin决定。属于同一个BFC的两个**相邻Box的`margin`会发生叠加**。

3.  每个元素的margin box的左边，
    **与包含块`border box`的左边相接触**，即使存在浮动也是如此。

4.  `BFC`的区域不会与`float box`叠加。

5.  `BFC`就是页面上的一个**隔离的独立容器**，容器里面的子元素不会影响到外面的元素，反之亦然。

6.  计算BFC的高度时，**浮动元素也参与计算**。

**创建块级格式化上下文**:

1.  浮动(元素的 float不为 none）

2.  绝对定位元素 (元素的 position为 absolute 或 fixed)

3.  行内块inline-blocks (元素的 display: inline-block)

4.  表格单元格 (元素的 display: table-cell，HTML表格单元格默认属性)

5.  表格标题(元素的 display: table-caption，HTML表格标题默认属性)

6.  overflow的值不为 visible的元素(元素的 overflow: hidden，overflow:
    auto)

7.  弹性盒子flex boxes (元素的 display: flex 或 inline-flex)

**应用场景**:

1.  自适应两栏布局

2.  清除内部浮动

3.  防止垂直margin重叠

12.子盒子在父盒子中水平垂直居中有几种方法？（定位、弹性盒子）
-------------------------------------------------------------

1.  定位position+margin外边距（需要知道子盒子的宽高）

        /*父盒子*/
        position:relative;

        /*子盒子*/
        position:absolute;
        top:50%;
        left:50%;
        margin-top:-50px;
        margin-left:-50px;

2.  定位position+transform（不需要知道宽高）

    这个方式虽然可变高度且代码量少，但是IE8不支持此方法，属性需要写浏览器厂商前缀而且可能干扰其他transform效果。

        /*父盒子*/
        position:relative;

        /*子盒子*/
        position:absolute;
        top:50%;
        left:50%;
        transform:translate(-50%,-50%);

3.  使用弹性盒子flex布局（不需要知道宽高）

        /*父盒子*/
        display:flex;
        justify-content;center;
        align-items:center;

4.  margin:auto（不需要知道宽高）

设置margin自动适应，然后设置定位的上下左右都为0，就如四边均衡受力从而实现盒子的居中;

        position:absolute;
        left:0;
        right:0;
        top:0;
        bottom:0;
        margin:auto;

1.  table-cell

将父盒子设置为table-cell(能够使元素呈单元格的样式显示)，并设置text-align:
center(使内容水平居中)；vertical-align:
middle(使内容垂直居中);。子盒子设置为inline-block可以使其内容变为文本格式，也可设置宽高；

13.css中怎么使盒子上下居中?
---------------------------

方法一:

设置垂直居中的时候要注意，先给**祖先元素`html`和`body`的高度**设置为100%；并且**清除默认样式**（`margin`和`padding`都设置为0，否则浏览器就会出现滚动条），然后给**子盒子相对定位和位移**`position:relative;top:50%;`，然后**减去本身宽度的一半**即可（`margin-top:负自身高度的一半，在不知道高度的情况下可以用：transform:translateY(-50%)`）向上偏移自身高度的一半；

方法二：

弹性盒子。给**父元素**设置`display:flex;align-items:center;`设置**body里的元素**垂直居中；（`justify-content:center;`定义body里的元素水平居中）

