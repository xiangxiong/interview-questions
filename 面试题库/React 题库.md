### React 类

虚拟dom 的原理:
1、用Javascript 对象结构表示DOM树的结构; 然后用这个树构建一个真正的DOM树,插入到文档当中。
2、当状态变更的时候，重新构造一颗新的对象树。然后用新的树和旧的树进行比较，记录两颗树的差异。
3、把2所记录的差异应用到步骤1所构建的真正的DOM树上,视图就更新了.

2、高阶组件用过吗?
高阶组件本质是一个高阶函数，最大的好处是解耦和灵活性.

怎么使用:
1、抽取公用代码和业务逻辑.功能和页面类似的页面，可以把一些共同的操作抽离到HOC组件中
2、抽离state. react 处理表单的时候，如果使用了受控组件，可以把onChange 事件同步改变state 的代码，封装到高阶组件中,还有提交的事件。
3、劫持渲染.(loading 动效,页面权限管理)。
4、操作props,增加用户信息属性对象.
5、反向继承. 场景:两个页面的相似度非常高，要新加一些属性的时候可以考虑使用.

3、继承和组合有什么区别?
继承的优点:继承的优点是子类可以重写父类的方法来方便实现对父类的扩展
集成的缺点:
1、父类的内部细节对子类是可见的.
2、子类从父类继承的方法在编译的时候就确定下来了，所以无法在运行期间改变从父类继承的方法和行为.
3、子类与父类是一种高耦合，违背了面向对象的思想
4、继承关系最大的弱点是打破了封装，子类能够访问父类的实现细节，子类与父类之间紧密耦合，子类缺乏独立性，从而影响了子类的可维护性
5、不支持动态继承，在运行时，子类无法选择不同的父类.

组合:
1、不破坏封装，整体类与局部类之间松耦合，彼此独立。
2、具有较好的可扩展性、
3、支持动态组合。在运行时,整体对象可以选择不同类型的局部对象.


4、React.Component 如何实现的?

Component vs PureComponent 什么区别?
1、PureComponent 继承了 Component。但是在原型链式扩展了 isPureComponent 的属性.
2、有 isPureComponent  的属性会进行浅比较.

浅比较是怎么实现的?
1、是比较两个对象是否相等. 源码是通过Object.is 方法来实现的. 比较的是对象是否相等. 但是没有比较多层嵌套的对象以及函数.
如何实现:
1、通过判断基本数据类型. Object.is 
2、判断两个对象的长度是否一致。
3、然后通过循环的方式判断两个对象是否相等

React.Component 里面有啥?
1、 setState.
2、forceUpdate.
3、state 的更新的入口.

React Hooks 诞生的背景是什么?
1、 Hook 是一些可以让你在函数组件里面使用state 以及生命周期等特性的函数。Hook 不能在class 中使用，以及更好的复用代码.
2、和 class 相比有哪些优点?
在这个 class 中，我们需要在两个生命周期函数中编写重复的代码. 而hooks 不需要.

react Hooks 解决什么问题?
在组件之间复用状态逻辑很难.
1、React并没有为组件提供一种可重复使用行为的方法（例如，将其关联到 store 里）。如果你已经使用了 React 一段时间，你可能会熟悉 render props(渲染属性) 和 高阶组件的模式尝试解决这个问题。但是这些模式要求您在使用它们时重构组件，这可能很麻烦并且使代码更难以跟踪。如果你看一下 React DevTools 中典型的 React 应用程序，你可能会发现一个包含提供者，消费者，高阶组件，渲染属性和其他抽象层的组件所包裹起来的组件。虽然我们可以 在 DevTools 中过滤它们 ，但这会引出了一个更深层次的基本问题：React 需要一个更好的原语来共享有状态逻辑.
2.使用 Hook ，您可以从组件中提取有状态逻辑，以便可以独立测试并重用。Hooks 允许您在不更改组件层次结构的情况下重用有状态逻辑.

复杂组件变得难以理解:
1、Hook 将组件中相互关联的部分拆分成更小的函数.
2、我们经常维护一些组件，组件起初很简单，但是逐渐会被状态逻辑和副作用充斥。每个生命周期常常包含一些不相关的逻辑。
3、组件常常在 componentDidMount 和 componentDidUpdate 中获取数据。但是，同一个 componentDidMount 中可能也包含很多其它的逻辑，如设置事件监听，而之后需在 componentWillUnmount 中清除。
4、而完全不相关的代码却在同一个方法中组合在一起。如此很容易产生 bug，并且导致逻辑不一致.

3、hooks 有哪些方法?
    1、useState,useEffect,useCallBack.

ref 用过吗?
1、用过, ref用于访问 render 方法中创建 dom 元素或者是 React 组件的实例.
2、ref 有三种用法, string ref,call back ref,createRef.
3、推荐使用:createRef.callback ref 不推荐使用 string ref.

为什么不推荐使用ref?
1、当 ref 定义为string 时,需要 React 追踪当前正在渲染的组件,在 reconciliation 阶段, React Element 创建
和更新的过程中, ref 会被包装为一个闭包函数, 等待 commit 阶段被执行，这会对React 的性能产生一些影响.
2、当使用 render callback 模式的时候,使用 string ref 会造成 ref 挂载位置产生歧义.

react 16  版本有哪些新的改变?

react 源码看过哪些?

redux 相关的问题:
1、中间件用过哪些?
redux thunk.


2、中间件使用的原理是什么?
中间件的原理是,它提供的是被发起之后，到达reducer 之前的扩展点.
将 store.dispatch 方法进行替换.

3、redux 基本原则是什么?
1、单一数据源.
2、State是只读的.
3、使用纯函数来执行修改.


### Javascript 类.

1、浏览器解析的过程
 (1). HTML 解析成 dom Tree StyleSheets 解析成 cssdomtree.两者AttachMent 合并成 RenderTree. RenderTree 包含的是Html,Css 但并不包含告诉浏览器哪个元素在哪个位置上面.通过Layout 才能真正的显示这个元素的宽高颜色，在RenderTree中显示出来.
Painting. 把元素渲染出来,Display. 把界面显示出来.

2、Jsonp 的原理.
利用script 标签的异步加载来实现的, 只能使用get 不能使用post.
定义有 jsonp 的全局函数. 然后通过回调函数来获取返回值.
function jsonp(req){
    var script = document.createElement('script');
    var url = req.url + '?callback=' + req.callback.name;
    script.src = url;
    document.getElementsByTagName('head')[0].appendChild(script); 
}
function hello(res){
    alert('hello ' + res.data);
}
jsonp({
    url : '',
    callback : hello 
});
3、原型链的原理.
创建对象有几种方法
instanceof 的原理.
new 运算符.

js 是如何实现继承的?
(1)、 原型继承模式.
(2)、借用构造函数
(3)、组合继承.

4、闭包.
闭包 = 函数 + 自由变量.

MDN 对闭包的定义为:

> 闭包是指那些能够自由变量的函数.

那什么是自由变量呢?

> 自由变量是指在函数中使用的, 但既不是函数参数也不是函数的局部变量的变量.

闭包 = 函数 + 函数能够访问的自由变量.

```
var a = 1;
function foo(){
	console.log(a);
}
foo();
```
实践的角度来说:
1、即使创建它的上下文已经销毁，它仍然存在(比如: 内部函数从父函数中返回).
2、在代码中引用了自由变量.
```js
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
var foo = checkscope();
foo();
```


5、eventLoop 异步.

什么是任务队列?
同步任务先执行.
异步任务后执行.

setTimeOut 和 setInterval
DOM 事件

Es6 中的Promise.
在Promise 的内部,有一个状态管理器的存在, 有三种状态: pending，fulfilled, rejected.
1、promise 对象初始化状态为peding.
2、当调用 resolve 会由 pending => fulfilled.
3、当调用 reject 会由 pending => rejected.

宏任务和微任务有哪些?
macrotasks: setTimeout setInterval setImmediate I/O UI渲染。
microtasks: Promise process.nextTick Object.observe MutationObserver.

Promise  比  setTimeout 要先执行.

因此，看上面的代码中resolve 其实是将promise 的状态由pending 改为 fullfilled,然后向
then 的成功回调函数传值，reject 反之. 但是注意 promise 状态，只能由pending => fuillid
一旦修改就不能改变.

当状态fulfilled 时, then 成功回调函数被调用,并接受上面传来的num. 进而进行操作,
promise.then 方法每次调用，都返回一个新的promise 对象 所以可以链式写法.

api：
all, race.

什么是EventLoop?
https://juejin.im/post/5ad3fa47518825619d4d3a11

7、node Steam 用过吗?

9、es6 的一些用法?


### CSS 类.
bfc:

position: static、fixed、absolute和relative的区别和用法
relatvie:
定位为relative的元素脱离正常的文本流中，但其在文本流中的位置依然存在。
无论父级存在不存在，无论有没有TRBL，均是以父级的左上角进行定位，但是父级的Padding属性会对其影响

absolute:
定位为absolute的层脱离正常文本流，但与relative的区别是其在正常流中的位置不再存在.
在Position属性值为absolute的同时，如果有一级父对象（无论是父对象还是祖父对象，或者再高的辈分，一样）的Position属性值为Relative时，则上述的相对浏览器窗口定位将会变成相对父对象定位，这对精确定位是很有帮助的

fixed（固定定位）：
生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。可通过z-index进行层次分级

static（静态定位）：默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）

https://www.cnblogs.com/theWayToAce/p/5264436.html

选择器优先级:
！important>行内样式>id选择器>类选择器>标签选择器>通配符>继承
display:none;与visibility:hidden的区别是什么？
(1) display:none; HTML元素（对象）的宽高，高度等各种属性值都将“丢失”,视为不存在，而且不加载
(2) visibility:hidden; HTML元素（对象）仅仅是在视觉上看不见（完全透明），而它所占据的空间位置仍然存在，也即是说它仍然具有高度，宽度等属性值.

BFC (边距重叠解决方案).
BFC的基本概念：块级格式化上下文.

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


清除浮动有哪些方式？
(1）在浮动元素下方添加一个非浮动元素
<div> <div style="float:left;width:45%;"></div> <div style="float:right;width:45%;"></div> <div style="clear:both;"></div> </div>

(2）将父容器也改成浮动定位
<div style="float:left;">
    <div style="float:left;width:45%;"></div>
    <div style="float:right;width:45%;"></div>
</div>

(3) 父容器设置overflow: hidden或者auto。
<div style="overflow: hidden;"> <div style="float:left;width:45%;"></div> <div style="float:right;width:45%;"></div> </div>

HTML5有哪些新特性，移除了哪些元素？
1、用于绘画的canvas元素。
2、用于媒介回放的video和audio元素；
3、对本地离线存储有更好的支持，localStorage长期存储数据，浏览器关闭后数据不丢失；sessionStorage的数据在浏览器关闭后自动删除。
4、语意化更好的内容元素，比如header,nav,section,article,footer.

css盒模型
1、基本概念: 标准模型+IE模型.
2、标准模型和IE模型区别.
标准模型：content.  IE: content+border+padding.

3、CSS如何设置这两种模型.
box-sizing:content-box; box-sizing:border-box;

行内元素和块级元素的区别？
	•	块级元素：div,p,h1,form,ul,li.
	•	行内元素：span,a,label,input,img,strong,em.

https://juejin.im/post/5b6d368b5188251b3c3b570b

css 选择器:
1、简单选择器。
通过元素类型、class 或 id 匹配一个或多个元素
2、属性选择器。
3、伪类。
4、伪元素。
5、组合器。
6、多用选择器。
https://segmentfault.com/a/1190000013424772


### 架构类.
* BFF.
* SeverLess 架构.
* Redux
* Dva.
* mobx.
* Rxjs.

### 小程序相关
* 小程序的基本原理.
* 

### 项目类
* 