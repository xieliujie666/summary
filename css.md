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

## 4.flex有哪些属性 

采用Flex布局的元素，称为Flex容器（flex container），简称”容器”。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称”项目”。 

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

**一、容器的属性**
以下6个属性设置在容器上。

```
1.flex-direction
    row（默认值）：主轴为水平方向，起点在左端。
    row-reverse：主轴为水平方向，起点在右端。
    column：主轴为垂直方向，起点在上沿。
    column-reverse：主轴为垂直方向，起点在下沿。
2.flex-wrap
    nowrap（默认）：不换行。
    wrap：换行，第一行在上方。
    wrap-reverse：换行，第一行在下方。

3.flex-flow？？？？？？？？？？？？？
  flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
.box {
  flex-flow: <flex-direction> || <flex-wrap>;}
  
4.justify-content 主轴上的对齐方式吧
    它可能取5个值，具体对齐方式与轴的方向有关。下面假设主轴为从左到右。
    flex-start（默认值）：左对齐
    flex-end：右对齐
    center： 居中
    space-between：两端对齐，项目之间的间隔都相等。
    space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔				大一倍。
5.align-items 交叉轴的对齐方式
    它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。
    flex-start：交叉轴的起点对齐。
    flex-end：交叉轴的终点对齐。
    center：交叉轴的中点对齐。
    baseline: 项目的第一行文字的基线对齐。
    stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
6.align-content？？？？？？？？？？？？？？？
    align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。 该属性可能取6个值。
    flex-start：与交叉轴的起点对齐。
    flex-end：与交叉轴的终点对齐。
    center：与交叉轴的中点对齐。
    space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
    space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
    stretch（默认值）：轴线占满整个交叉轴
```

**二、项目的属性**
以下6个属性设置在项目上。

```
1.order
    order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
    .item { order: <integer>;}

2.flex-grow
    flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
    .item {  flex-grow: <number>; /* default 0 */}
    如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

3.flex-shrink

    flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
    .item { flex-shrink: <number>; /* default 1 */}
    如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
    负值对该属性无效。
？？？？？？？？？？？？？？？？？？？？？？？？？？
4.flex-basis
    flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
    .item {
      flex-basis: <length> | auto; /* default auto */
    }它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

5.flex
    flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
    .item {
      flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
    }
    该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
    建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

6.align-self
    align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
    .item {
      align-self: auto | flex-start | flex-end | center | baseline | stretch;
    }该属性可能取6个值，除了auto，其他都与align-items属性完全一致
```



## 【高】flex 实现一个圣杯布局 @

圣杯布局就是所说的3栏布局, 左右定宽, 中间自适应。 

```
.content{
            height:300px;
            display: flex;
        }
        .left{ 
            height:100%;
             flex:0 0 200px;
            background-color: #00a0dc;
        }
        .middle{
            height:100%; 
            background-color: red;
            flex: 1;
        }
        .right{
            height:100%; 
            flex:0 0 200px;
            background-color: #00a0dc;
        }
```



## 5.居 中 的 方法@

**1.垂 直 居 中 的 方 法**

CSS中的确是有vertical-align属性，但是它只对(X)HTML元素中拥有valign特性的元素才生效，例如表格元素中的<td>、<th>、<caption>等，而像<div>、<span>这样的元素是没有valign特性的，因此使用vertical-align对它们不起作用 。

（0）单行文字实现垂直居中 ：设置它的实际高度height和所在行的高度line-height相等即可 。使用overflow:hidden的设置是为了防止内容超出容器或者产生自动换行，这样就达不到垂直居中效果了 。

**(1)margin:auto 法**

```
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
```

定位为上下左右为 0，margin：0 可以实现脱离文档流的居中.
**(2)margin 负值法**子绝父相

```
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
}transform：translateX(-50%)和 transform：translateY(-50%)
```

补充：也可以将 marin-top 和 margin-left 负值替换成transform: translate(-50%,-50%); 
**(3)table-cell**（未脱离文档流的）
设置父元素的 display:table-cell,并且 vertical-align:middle，子元素可以实现垂直居中。

```
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
```

**(4)利用 flex**
将父元素设置为 display:flex，并且设置 align-items:center;justify-content:center;

```
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
```

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

## 8.两栏布局

1.**利用float配合margin** 

2.**利用position配合margin**  

3.**display:flex与flex-grow**

  display:flex需要给予父级，flex-grow放置自适应的子元素。（IE10一下不支持） 

```
 <style>
        .box{display: flex}
        .box div{
            height:100px;
        }
        .left{
            width:200px;
            background-color:pink;
        }
        .right{
            flex-grow:1; //自动将你剩下的补全
            background-color:yellow
        }
    </style>
```

4.**display:table和display:table-cell**  

display:table 给予父级，display:table-cell给予两个子集  .box{display: table}

```
       .box div{
            display: table-cell
        }
        .left{
            min-width:200px;
            background-color:pink;
        }
        .right{
            width:100%;
            background-color:yellow;
        }
```

## 9.vm、em、px、rem 

10.1px实现 





# 二、移动端

1. rem和em
2. 响应式布局
3. css动画的实现
4. [前端](https://www.nowcoder.com/jump/super-jump/word?word=%E5%89%8D%E7%AB%AF)无障碍设计(不是很懂)
5. scss和less有没有了解过，(回答了解一点less),less的优缺点
6. flex布局 baseline是怎么计算的 