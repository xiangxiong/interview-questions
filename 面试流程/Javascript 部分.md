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
(1)、原型继承模式.
Child.prototype = new Parent();
父类对象去赋值给子类的原型.

缺点:
引用类型的属性被所有实例共享.
在创建 Child 的实例时，不能向Parent传参

缺点:
1.避免了引用类型的属性被所有实例共享.
2.可以在 Child 中向 Parent 传参.


(2)、借用构造函数

function Child () {
    Parent.call(this);
}
var child1 = new Child();

缺点:
方法都在构造函数中定义，每次创建实例都会创建一遍方法.

(3)、组合继承.
Child.prototype = new Parent();
var child1 = new Child('kevin', '18');
child1.colors.push('black');

优点: 融合原型链继承和构造函数的优点，是 JavaScript 中最常用的继承模式.

缺点:
组合继承最大的缺点是会调用两次父构造函数。

(4)、寄生组合式继承

// 关键的三步
var F = function () {};

F.prototype = Parent.prototype;

Child.prototype = new F();

var child1 = new Child('kevin', '18');

创建一个新的对象， 用父类的原型对象指向新的对象。 用子类的原型指向新的对象。

这种方式的高效率体现它只调用了一次 Parent 构造函数，并且因此避免了在 Parent.prototype 上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用 instanceof 和 isPrototypeOf。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。。

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

你必须意识到的第一件事是函数内部this的值不是静态的，每次你调用一个函数它总是重新求值，但这一过程发生在函数代码实际执行之前。函数内部的this值实际由函数被调用的父作用域提供，更重要的是，依赖实际函数的语法.
当函数被调用时，我们看紧邻括号“()”的左边。如果在括号的左侧存在一个引用，传递给调用函数的“this”值是引用属于的对象，否则this的值将是全局对象.

```
function bar() {
    alert(this);
}
bar(); // global - 因为bar方法被调用时属于 global 对象

var foo = {
    baz: function() {
        alert(this);
    }
}
foo.baz(); // foo - 因为baz()方法被调用时术语foo对象
```

> JavaScript中的this关键字
https://yanhaijing.com/javascript/2014/04/30/javascript-this-keyword/

this的工作原理
如果一个函数被作为一个对象的方法调用，那么this将被指派为这个对象。


> JavaScript的作用域和提升机制
https://yanhaijing.com/javascript/2014/04/30/JavaScript-Scoping-and-Hoisting/

> 了解JavaScript的执行上下文
https://yanhaijing.com/javascript/2014/04/29/what-is-the-execution-context-in-javascript/

什么是执行上下文？

全局代码——你的代码首次执行的默认环境。
函数代码——每当进入一个函数内部。
Eval代码——eval内部的文本被执行时。

激活/变量对象【AO/VO】


下面是解释器如果执行代码的伪逻辑：

查找调用函数的代码。
执行函数代码之前，先创建执行上下文。
进入创建阶段：  
    初始化作用域链：
    创建变量对象：
    创建arguments对象，检查上下文，初始化参数名称和值并创建引用的复制。
    扫描上下文的函数声明：
    为发现的每一个函数，在变量对象上创建一个属性——确切的说是函数的名字——其有一个指向函数在内存中的引用。
    如果函数的名字已经存在，引用指针将被重写。
    扫面上下文的变量声明：
    为发现的每个变量声明，在变量对象上创建一个属性——就是变量的名字，并且将变量的值初始化为undefined
    如果变量的名字已经在变量对象里存在，将不会进行任何操作并继续扫描。
    求出上下文内部“this”的值。
激活/代码执行阶段：
    在当前上下文上运行/解释函数代码，并随着代码一行行执行指派变量的值。



提升（Hoisting）

```
(function() {
    console.log(typeof foo); // 函数指针
    console.log(typeof bar); // undefined
    var foo = 'hello',
        bar = function() {
            return 'world';
        };
    function foo() {
        return 'hello';
    }
}()); ​ 我们能回答下面的问题
```


为什么我们能在foo声明之前访问它？
如果我们跟随创建阶段，我们知道变量在激活/代码执行阶段已经被创建。所以在函数开始执行之前，foo已经在活动对象里面被定义了。
Foo被声明了两次，为什么foo显示为函数而不是undefined或字符串？
尽管foo被声明了两次，我们知道从创建阶段函数已经在活动对象里面被创建，这一过程发生在变量创建之前，并且如果属性名已经在活动对象上存在，我们仅仅更新引用。
因此，对foo()函数的引用首先被创建在活动对象里，并且当我们解释到var foo时，我们看见foo属性名已经存在，所以代码什么都不做并继续执行。
为什么bar的值是undefined？
bar实际上是一个变量，但变量的值是函数，并且我们知道变量在创建阶段被创建但他们被初始化为undefined。

> 原型链.

https://coding.imooc.com/lesson/129.html#mid=6338