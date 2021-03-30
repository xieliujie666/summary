## 1.css盒模型

CSS中的盒子模型包括IE盒子模型和标准的W3C盒子模型。

box-sizing(有3个值哦)：border-box,padding-box,content-box.

标准盒子模型：

![2018-07-10 4 24 03](C:\Users\superssssss\Desktop\Interview\mj-images\42498021-b4dd6a46-845d-11e8-8bd9-ac2d90985f2a.png) 

IE盒子模型：

![2018-07-10 4 24 12](C:\Users\superssssss\Desktop\Interview\mj-images\42498075-d3496e3a-845d-11e8-919c-eb3a7866883b.png) 

区别：从图中我们可以看出，这两种盒子模型最主要的区别就是width的包含范围，在标准的盒子模型中，width指content部分的宽度，在IE盒子模型中，width表示content+padding+border这三个部分的宽度，故这使得在计算整个盒子的宽度时存在着差异：

标准盒子模型的盒子宽度：左右border+左右padding+width
IE盒子模型的盒子宽度：width

在CSS3中引入了box-sizing属性，box-sizing:content-box;表示标准的盒子模型，box-sizing:border-box表示的是IE盒子模型

最后，前面我们还提到了，box-sizing:padding-box,这个属性值的宽度包含了左右padding+width

也很好理解性记忆，包含什么，width就从什么开始算起。

## 2.选择器的优先级 

当两个规则都作用到了同一个html元素上时，如果定义的属性有冲突，那么应该用谁的值的，CSS有一套优先级的定义。

**不同级别**（!CSS优先级)由四个级别和各级别的出现次数决定的。每个规则对应个初始"四位数"

1. 在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式。
2. 行内样式（作为style属性写在元素内)1000
3. id选择器                                                 0100
4. 类选择器                                                 0010
5. 标签选择器                                              0001
6. 通配符选择器
7. 浏览器自定义或继承

   总结排序：!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性

**同一级别**

同一级别中后写的会覆盖先写的样式(!CSS层叠性，另外!CSS继承性)

## 3.position 的取值 

1、absolute：

生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。

2、fixed：

生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。

3、relative：

生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。

4、static：

默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。

## 4.【高】flex 实现一个圣杯布局 ?？？

## 5.居 中 的 方法？文字

1.垂 直 居 中 的 方 法

(1)margin:auto 法
css:
div{
width: 400px;
height: 400px;
position: relative;
border: 1px solid #465468;
}
img{
position: absolute;
margin: auto;
top: 0;
left: 0;
right: 0;
bottom: 0;
}
html:
<div><img src="mm.jpg"></div>

定位为上下左右为 0，margin：0 可以实现脱离文档流的居中.
(2)margin 负值法
.container{
width: 500px;
height: 400px;
border: 2px solid #379;
position: relative;
}
.inner{
width: 480px;
height: 380px;
background-color: #746;
position: absolute;
top: 50%;
left: 50%;
margin-top: -190px; /*height 的一半*/
margin-left: -240px; /*width 的一半*/
}
补充：其实这里也可以将 marin-top 和 margin-left 负值替换成，
transform：translateX(-50%)和 transform：translateY(-50%)
(3)table-cell（未脱离文档流的）
设置父元素的 display:table-cell,并且 vertical-align:middle，这样子元素可以实现垂直居中。
css:
div{
width: 300px;
height: 300px;
border: 3px solid #555;
display: table-cell;
vertical-align: middle;
text-align: center;
}
img{
vertical-align: middle;
}
(4)利用 flex
将父元素设置为 display:flex，并且设置 align-items:center;justify-content:center;
css:
.container{
width: 300px;
height: 200px;
border: 3px solid #546461;
display: -webkit-flex;
display: flex;
-webkit-align-items: center;
align-items: center;
-webkit-justify-content: center;
justify-content: center;
}
.inner{
border: 3px solid #458761;
padding: 20px;

## 6.img标签 

src:图像的路径

alt：图像不能显示时的替换文本

title：鼠标悬停时显示的内容

## 7.margin百分比计算

对元素的margin设置百分数时，百分数是相对于父元素的width计算，不管是margin-top/margin-bottom还是margin-left/margin-right。当然，padding的原理也是一样的。

如果没有为元素声明width，此时元素框的总宽度包括外边距取决于父元素的width，这样可能得到“流式”页面，即元素的外边距会扩大或缩小以适应父元素的实际大小。

为什么margin-top/margin-bottom的百分数是相对于width而不是height呢？

CSS权威指南中的解释：

我们认为，正常流中的大多数元素都会足够高以包含其后代元素（包括外边距），如果一个元素的上下外边距时父元素的height的百分数，就可能导致一个无限循环，父元素的height会增加，以适应后代元素上下外边距的增加，而相应的，上下外边距因为父元素height的增加也会增加，如此循环。



# 二、移动端

1. rem和em
2. 响应式布局
3. css动画的实现
4. [前端](https://www.nowcoder.com/jump/super-jump/word?word=%E5%89%8D%E7%AB%AF)无障碍设计(不是很懂)
5. scss和less有没有了解过，(回答了解一点less),less的优缺点
6. flex布局 baseline是怎么计算的 