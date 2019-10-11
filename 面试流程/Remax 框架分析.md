### Remax 框架分析.


### 参考文献:
1、https://zhuanlan.zhihu.com/p/54795961.
2、https://zhuanlan.zhihu.com/p/79788488.
3、https://zhuanlan.zhihu.com/p/54217391.


如何写出高质量的组件库:

Remax 框架的基本原理:
Remax 的实现原理和基于静态编译的方案有所不同，其核心其实是重新实现了 ReactDOM 的部分.
众所周知，React 本身的设计就是支持跨端渲染的，render 部分和 React 的核心逻辑是解耦的（甚至不在一个 npm 包里）。主要的 render 有 ReactDOM（浏览器），ReactDOMServer（服务器端）和 ReactNative.
Remax 要做的事情和 ReactNative 要做的事情非常类似，我们重新接管了 ReactDOM 的 render。
在原有的 React 页面中，React 在完成 Diff 发现需要修改界面时，又 ReactDOM 把改变 Patch 到页面上。
而在小程序中由于我们不能直接修改页面，则由 React 完成 DIFF 后由 Remax 把修改 Patch 到内存中的虚拟 DOM 上，然后再通过小程序自己的虚拟 DOM 最后把改变同步到页面上.
在这里我把这个过程说得非常简单，但实际上是有些坑要填的，主要也都是来自于小程序的限制，后续会有新的文章展开来讲。但是这种实现方式使得我们完全可以把 React 的代码放在小程序的环境中运行.

现有的方案.
目前社区中使用 React 构建小程序的方案大都使用静态编译的方式实现。所谓静态编译，就是使用工具把代码语法分析一遍，把其中的 JSX 部分和逻辑部分抽取出来，分别生成小程序的模板和 Page 定义.

这种方案最大的问题就是会有很多限制，因为语法分析是静态的，所以这些方案都会去限制一些动态的写法。另外正是因为 JavaScript 语言的动态性，要去做语法分析本身就是件很复杂的事情，所以这些方案实现起来往往也非常复杂.

最重要的,静态编译后你的代码就转换成了小程序代码，运行时其实并没有 React 的存在，你只是用了 React 的写法， 而不是真正的在用 React 写应用.

类似于react-native 方案.


总结:


https://zhuanlan.zhihu.com/p/54795961


ReactDOM 
https://itbilu.com/javascript/react/EJiqU81te.html.
