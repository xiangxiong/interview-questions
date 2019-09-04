> position: static、fixed、absolute和relative的区别和用法

relatvie:
定位为relative的元素脱离正常的文本流中，但其在文本流中的位置依然存在。
无论父级存在不存在，无论有没有TRBL，均是以父级的左上角进行定位，但是父级的Padding属性会对其影响

absolute:
定位为absolute的层脱离正常文本流，但与relative的区别是其在正常流中的位置不再存在.
在Position属性值为absolute的同时，如果有一级父对象（无论是父对象还是祖父对象，或者再高的辈分，一样）的Position属性值为Relative时，则上述的相对浏览器窗口定位将会变成相对父对象定位，这对精确定位是很有帮助的

fixed（固定定位):
生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。可通过z-index进行层次分级

static（静态定位）：默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明)

https://www.cnblogs.com/theWayToAce/p/5264436.html

> CSS 优先级:
!important>行内样式>id选择器>类选择器>标签选择器>通配符>继承.

> Css 选择器:
css 选择器:
1、简单选择器。
通过元素类型、class 或 id 匹配一个或多个元素
2、属性选择器。
如果希望选择有某个属性的元素，而不论属性值是什么，可以使用简单属性选择器.
*[title] {color:red;}

3、伪类。
:active
:any
:checked
:default
:dir()
:disabled
:empty
:enabled
:first
4、伪元素。

5、组合器。
section > p {
  background-color: yellow;
}

h2 + p {
  text-transform: uppercase;
}
允许您选择一个元素，它是另一个元素的直接兄弟元素
h2 ~ p {
  border: 1px dashed black;
}
允许您选择其他元素的兄弟元素
6、多用选择器。
https://segmentfault.com/a/1190000013424772

暂停：重新整理上面的知识点,模拟面试 1h.

>  display:none;与visibility:hidden的区别是什么？
(1) display:none; HTML元素（对象）的宽高，高度等各种属性值都将“丢失”,视为不存在，而且不加载
(2) visibility:hidden; HTML元素（对象）仅仅是在视觉上看不见（完全透明），而它所占据的空间位置仍然存在，也即是说它仍然具有高度，宽度等属性值.


> BFC (边距重叠解决方案).
BFC的原理（渲染规则）,IFC：
1、处于同一个BFC中的元素相互影响，可能会发生margin collapse.(边距重叠) (父子元素的边界重叠,兄弟元素的边界重叠.（垂直方向）,空元素的边界重叠)
2、bfc的区域不会与浮动元素的box重叠.
3、bfc在页面上是一个独立的容器.(外面的元素，不会影响里面的元素.).
4、计算bfc高度时，浮动元素也会参与计算.(BFC子元素即使是float也会参与计算.)
记忆法关键字：渲染规则.

如何创建BFC?
1、浮动 (float的值不为none)；
2、绝对定位元素（position的值为absolute或fixed）；
6、overflow (溢出) 不为 visible.
4、表格单元（display为table、table-cell、table-caption等HTML表格相关属性）；
3、行内块 (display为inline-block)
5、弹性盒 (display为flex或inline-flex)

> 清除浮动有哪些方式?
(1）在浮动元素下方添加一个非浮动元素
<div>
<div style="float:left;width:45%;"></div> <div style="float:right;width:45%;"></div> <div style="clear:both;"></div>
</div>

(2）将父容器也改成浮动定位
<div style="float:left;">
    <div style="float:left;width:45%;"></div>
    <div style="float:right;width:45%;"></div>
</div>

(3) 父容器设置 overflow:hidden 或者 auto.
<div style="overflow: hidden;"> <div style="float:left;width:45%;"></div> <div style="float:right;width:45%;"></div> </div>

> HTML5有哪些新特性，移除了哪些元素？

1、用于绘画的canvas元素。
2、用于媒介回放的video和audio元素;
3、对本地离线存储有更好的支持，localStorage长期存储数据，浏览器关闭后数据不丢失；sessionStorage的数据在浏览器关闭后自动删除。
4、语意化更好的内容元素，比如header,nav,section,article,footer.


> css盒模型
1、基本概念: 标准模型+IE模型.
2、标准模型和IE模型区别.
标准模型：content.  IE: content+border+padding.

3、CSS如何设置这两种模型.
box-sizing:content-box; box-sizing:border-box;

行内元素和块级元素的区别？
	•块级元素：div,p,h1,form,ul,li.
	•行内元素：span,a,label,input,img,strong,em.

-----

SASS, LESS - 提供了变量、简单函数、运算、继承等，扩展性、重用性都有了很大的提升，解决了一些样式重用冗余的问题，但是对于命名混乱问题的效果不大。

BEM (.block__element--modifier) - 比较流行的class命名规则，部分解决了命名混乱和全局污染的问题，但class定义起来还是不太方便，比较冗长，而且和第三方库的命名还是有可能冲突。

CSS Modules - 模块化CSS，将CSS文件以模块的形式引入到JavaScript里，基本上解决了全局污染、命名混乱、样式重用和冗余的问题，但CSS有嵌套结构的限制（只能一层），也无法方便的在CSS和JavaScript之间共享变量.

-----
```

全局污染 - CSS的选择器是全局生效的，所以在class名称比较简单时，容易引起全局选择器冲突，导致样式互相影响.

命名混乱 - 因为怕全局污染，所以日常起class名称时会尽量加长，这样不容易重复，但当项目由多人维护时，很容易导致命名风格不统一.

样式重用困难 - 有时虽然知道项目上已有一些相似的样式，但因为怕互相影响，不敢重用.

代码冗余 - 由于样式重用的困难性等问题，导致代码冗余.

```