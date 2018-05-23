 # 概述
 本章节 
首先要理解：  
 css 的各种属性并不都是独立的生效的，而是存在互相依赖的，以一个span 为例，只有在修改了，display之后，再设置width才会生效，

 # CSS的选择器
 * 类选择器
* ID选择器
* 属性选择器
* 伪类选择器  用 :  连接
* 伪元素选择器   用  :: 连接

* 关系选择器：
    * 后代选择器  选择所有合乎规则的后代元素， 使用空格连接
    * 相邻后代选择器    使用 > 连接
    * 兄弟选择器   使用 ~ 连接   IE7 以上
    * 相邻兄弟选择器  使用功能 + 连接  IE7 以上
    
# 流、元素与基本尺寸
元素分为两类：块级元素和内联元素

块级元素对应英文为block-level element 常见块级元素有 div li table 


块级元素 和 display为block的元素并不是一个概念  li元素默认display是list-item table的display为table 他们均为块级元素 
## 也就是水平流上只能单独显示一个元素，多个块级元素则会换行显示
由于块级元素具有换行特性，所以可以配合 clear来清除浮动

每个元素有两个盒子，外层元素负责是一行显示，还是只能换行显示，内侧盒子负责宽高内容呈现什么的，即容器盒子

例如inline-block可以视为外层盒子为 inline 所以可以 多个元素一行显示，内在盒子为block 所以还可以设置宽高

所以width和height作用在了内在盒子上

## 关于width：auto
width的默认值为auto  它包含了一下四种不同宽度表现
>（1）充分利用可利用空间 例如div p 等元素的宽度默认为 100% 于父级容器的

>（2）收缩与包裹 代表为浮动、绝对定位、inline-block、table 英文名为shrink-to-fit 即收缩到合适

>（3）收缩到最小 最容易出现在table-layout为auto的table中

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















