Javascript 相关的问题:

> 1、浏览器解析的过程.

 (1). HTML 解析成 dom Tree StyleSheets 解析成 cssdomtree.两者AttachMent 合并成 RenderTree. RenderTree 包含的是Html,Css 但并不包含告诉浏览器哪个元素在哪个位置上面.通过Layout 才能真正的显示这个元素的宽高颜色，在RenderTree中显示出来.
Painting. 把元素渲染出来,Display. 把界面显示出来.


> 2、Jsonp 的原理.

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

> 3、原型链的原理.
创建对象有几种方法
instanceof 的原理.
new 运算符.

> 4、js 是如何实现继承的?
(1)、 原型继承模式.
(2)、借用构造函数
(3)、组合继承.

> 5、闭包.
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

> 5、eventLoop 异步.

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

> 认识javascript中的作用域和上下文
https://yanhaijing.com/javascript/2013/08/30/understanding-scope-and-context-in-javascript/

> 揭秘JavaScript中谜一样的this
https://yanhaijing.com/javascript/2013/12/28/demystifying-this-in-javascript/

> JavaScript中的this关键字
https://yanhaijing.com/javascript/2014/04/30/javascript-this-keyword/


> JavaScript的作用域和提升机制
https://yanhaijing.com/javascript/2014/04/30/JavaScript-Scoping-and-Hoisting/

> 了解JavaScript的执行上下文
https://yanhaijing.com/javascript/2014/04/29/what-is-the-execution-context-in-javascript/

> 原型链
https://coding.imooc.com/lesson/129.html#mid=6338