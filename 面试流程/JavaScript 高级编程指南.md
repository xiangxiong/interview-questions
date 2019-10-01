### 基本概念

* 数据类型.
    * Typeof 检测检测给定变量的数据类型.
    * Undefined 类型.
        * Undefined 类型只有一个值,既特殊的undefined. 在使用var 声明变量但未对其加以初始化时,这个变量的值就是undefined.

    * NULL 类型.
        Null 类型是第二个只有一个值的数据类型,这个特殊值是null. 从逻辑角度来看,null 值表示一个空对象指针,而这也是
        使用typeof 操作符检测null 值时会返回 object 的原因. 

    * Boolean 类型.
        任何数据类型 通过 Boolean 类型转换之后 会变成 true,false.
        在条件判断时，除了 undefined， null， false， NaN， ''， 0， -0，其他所有值都转为 true，包括所有对象。

        所有对象的布尔值都是true，甚至连false对应的布尔对象也是true。
        请注意，空对象{}和空数组[]也会被转成true

    * Number 类型
         (浮点数值在某些语言中存在双精度数值.) 数值的范围为 八进制,十六进制.
         对象那个

        浮点数据类型:
        记忆:正确数据类型的就是原来值,如果里面是各种类型的就是Null,如果是空就是0.
        undefined 转为 NaN.  如果是null 就是0. bool 值, false 就是0.
        parseInt 有两个参数, 第一个参数是实际的值,第二个参数是要转换的进制.10进制,12进制,16进制.

    * String 类型.

    * Object 类型.

    类型转换:
        隐式类型转换.
            四则运算
            只有当加法运算时，其中一方是字符串类型，就会把另一个也转为字符串类型。其他运算只要其中一方是数字，那么另一方就转为数字。并且加法运算会触发三种类型转换：将值转换为原始值，转换为数字，转换为字符串.

* 面试题目:
    1、基本数据类型的考察.

* 变量、作用域和内存的问题.
    * instanceof.
    * 基本类型和引用类型
    * 执行环境和作用域.

* 引用类型.
    * 

* 面向对象的程序设计.
    * 创建对象的方式:
    ```
        字面量
        var o2 = new Object({name:"02"});
        // 第二种方式: 通过构造函数
        var M = function(name){ console.log(name)};
        var o3 = new M('o1');
        var  p = {name:"p"} 
        var op = object.create(p)
    ```

    ```
    function M(name){console.log(name)} ;
    var o3 = new M('coco');
    // M.prototype.constructor === M
    // o3.__proto__ === M.prototype
    // M.__proto__ === Function.prototype

    最顶端 object.prototype.
    ```
    instanceof 的原理.
    ```
    判断实例对象的__proto__ 和 prototype 是不是同一个引用.
    o3.__proto__ === M.prototype
    construnct.
    ```

继承的集中方式:
1、原型链继承.
引用类型的属性被所有实例共享
2、借用构造函数.
3、组合继承
4、原型式继承.
5、寄生式继承.
6、寄生组合式继承.

* 函数表达式.
闭包使用场景:
1、可以模仿块级作用域。()
2、定义私有变量和特权方法.
3、可以模块模式.

ES6 用过哪些?
1、set 和 map 结构.
  map: 是一种键值对的集合，只能用字符串作为键.
  set: 类似于数组,但成员的值都是惟一的,没有重复.

2、async 函数.
  async 函数实现的原理就是 promise 的reslove.

3、Generator.
4、扩展运算符.
... 

5、函数的扩展.
   箭头函数.
   rest 参数.
6、import,export.
7、class 继承.
  static 实际上是类的一个熟悉，es5 是函数的一个属性. 而不是实例的属性.

8、Promise 对象.
10、变量的解构赋值.
 变量解构 let [a, b, c] = [1, 2, 3];
 对象结构 let {} = this.props;

11、let 和 const 和 var

12、对象的扩展.
Object.is
Object.assgin()

css 3 最新的特性有哪些?
1、CSS3的选择器
E: last-child 匹配父元素的最后一个子元素E.
E: nth-child(n)匹配父元素的第n个子元素E.
E: nth-last-child(n) CSS3 匹配父元素的倒数第n个子元素E.

CSS3实现圆角（border-radius:8px)
阴影（box-shadow:10px）， box-shadow: 10px 10px 5px #888888;
对文字加特效（text-shadow: 2px 2px #ff0000;) (text-shadow属性连接一个或更多的阴影文本)
线性渐变（gradient）:CSS3 渐变（gradients）可以让你在两个或多个指定的颜色之间显示平稳的过渡.
旋转（transform）
transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);//旋转,缩放,定位,倾斜

H5 有哪些新的特性?

```
用于绘画的 canvas 元素
用于媒介回放的 video 和 audio 元素
对本地离线存储的更好的支持
新的特殊内容元素，比如 article、footer、header、nav、section
新的表单控件，比如 calendar、date、time、email、url、search
```

你在项目里面做了哪些改进?
1、引入新的技术进入到项目里面来.
react hooks.

2、webpack 技术到项目里面.
webpack 构建速度优化&体积优化.
预编译资源编译模块  DLLPlugin 进行分包
多进程/多实例构建:资源并行解析可选方案. HappyPack 方案.
webpack 4 改造.

3、引入taro 来写小程序的尝试.

我们平时的工作是按业务线来划分的比如：我负责用户中心,海外地产,商业地产 每个业务线对应相应的产品，基本日常就是 产品提需求，我们参与评审，提出技术方案，然后排期，开发，上线，测试.
跟踪业界的一些新的技术动态，对现有项目的改造.
每周会有一些分享. 平时遇到的一些问题.

项目上的问题:
1、项目上的问题.

你是如何学习的?
一般会建立自己的一套体系。 可以分为横向和纵向来区分.
比如在学react 这么知识.  

为什么要用react 来开发，了解诞生的背景?
react 主要用来解决什么问题?
react 实现的基本原理是什么?
react 这种框架和同类框架上有什么不同? 优缺点是什么?
react 如何引入到我们的项目中来，如何改善我们现有产品的体验?
react 常用的api 有哪些? 生态建设怎么样?





        
