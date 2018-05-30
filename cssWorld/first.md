# 概述

 本章节

首先要理解：

     css 的各种属性并不都是独立的生效的，而是存在互相依赖的，以一个span 为例，只有在修改了，display之后，再设置width才会生效，

## CSS的选择器

* 类选择器
* ID选择器
* 属性选择器
* 伪类选择器  用 :  连接
* 伪元素选择器   用  :: 连接

* 关系选择器：
* * 后代选择器  选择所有合乎规则的后代元素， 使用空格连接
* * 相邻后代选择器    使用 > 连接
* * 兄弟选择器   使用 ~ 连接   IE7 以上
* * 相邻兄弟选择器  使用功能 + 连接  IE7 以上

## 流、元素与基本尺寸

元素分为两类：块级元素和内联元素

块级元素对应英文为block-level element 常见块级元素有 div li table

块级元素 和 display为block的元素并不是一个概念  li元素默认display是list-item table的display为table 他们均为块级元素

## 也就是水平流上只能单独显示一个元素，多个块级元素则会换行显示由于块级元素具有换行特性，所以可以配合 clear来清除浮动

每个元素有两个盒子，外层元素负责是一行显示，还是只能换行显示，内侧盒子负责宽高内容呈现什么的，即容器盒子

例如inline-block可以视为外层盒子为 inline 所以可以 多个元素一行显示，内在盒子为block 所以还可以设置宽高

所以width和height作用在了内在盒子上

## 关于width：auto

width的默认值为auto  它包含了一下四种不同宽度表现
>（1）充分利用可利用空间 例如div p 等元素的宽度默认为 100% 于父级容器的

---

>（2）收缩与包裹 代表为浮动、绝对定位、inline-block、table 英文名为shrink-to-fit 即收缩到合适

---

>（3）收缩到最小 最容易出现在table-layout为auto的table中

---

>（4）超出容器限制  除非有明确的width限制否则上面三种都不会主动超过父级容器的限制的，，但是存在特殊情况 例如内容很长的连续英文或数字或者内联元素被设置了，white-space：nowrap

* 正常流宽度

当给元素设置 display:block 的时候就无需再去指定width为100%了

* 格式化宽度

格式化宽度仅出现在“绝对定位模型中”，也就是position为absolute或者fixed的元素中，宽度一般有内部尺寸决定，

对于非替换元素当left/right或者top/bottom同时存在的时候，表现为格式化宽度，其宽度大小相对于最近的具有定位特性的（position不是static）的祖先元素计算

```css
div{
    position:absolute;
    left:20px;
    right:20px;
}
```

假设该div最近的具有定位特性的元素宽度为1000px则这个元素的宽度为 960px

margin、border、padding、content同样会自动水平分配

## 内部尺寸与流体特性

简单来说就是元素的尺寸由内部元素决定，而非外部容器决定

* （1）包裹性

button按钮能够自动换行，
input不能，input默认值white-space:pre需要重置为normal

外部宽度较小的时候
width：auto;元素宽度为首选最小宽度

margin-box border-box padding-content content-box

css流体布局下的宽度分离原则，也就是width属性和border、padding等影响实际显示宽度的属性，不共存，让width单独占据一层标签

box-sizing直译为“盒尺寸呢”，实际上是“盒尺寸的作用细节”即‘width的作用细节’，content-box，padding-box，border-box，margin-box
content-box,border-box全都支持，

## height

对于height属性，如果父元素的height属性是auto，在子元素中的height属性的百分比会被忽略

height百分比要生效，父级元素中必须有一个可以生效的高度值。

让元素支持height：100%

1、显示设置高度值

2、使用绝对定位

min-width\min-height的初始值是 auto
max-width\max-width的初始值是 none

超越 !important max-width的值会覆盖，width
min-width的值会覆盖max-width的值

由于移动端对CSS动画支持良好，所以移动端的框架是没有动画模块的，有时候要动态展开的内容不是固定的，所以高度也不是定值，而是auto，从0-->auto是无法计算的，因此无法形成过渡动画效果，这个时候要借助max-width来实现，

## 盒尺寸的四大家族

### content box、padding box、border box、 margin box

* 深入理解content

content 与替换元素，

>什么替换元素
根据外在盒子是内联还是块级，把元素分为内联元素，和块级元素，而根据是否具有可替换内容，分为替换元素和非替换元素,
---
>替换元素顾名思义内容可以被替换，例如

```html
<img src="1.jpg">
```

如果我们将src修改，则图片就会被替换，这种通过修改某个属性值，呈现内容就可以被替换的元素，就被称为替换元素，因此，`<img>、<object>、<video>、<iframe>、<textarea>、<input>`,都是典型的替换元素

替换元素除了内容可替换之外，还有以下特性，

（1）内容的外观不收页面css影响

（2）有自己的尺寸，

很多替换元素在没有明确尺寸设定的情况下，其默认尺寸是，300*150像素，不包括边框，也有少部分元素是0像素，例如`<img>`

（3）在很多css属性上有自己的一套规则

例如vertical-align属性对于替换元素和非替换元素属性值解释是不一样的，