# interview-questions

interview-questions

## 计划:
1、明天上午全部在复盘一次各个知识点.

开始时间:9月29日 9:30

自我介绍:
1、我是向雄,毕业于中南大学,目前就职于58集团-安居客二手房前端技术部，担任前端工程师.

面试开始:
1、先介绍一下你们做的项目以及技术架构?  过关的
2、React 基础知识?
    1、react 生命周期有哪些?
    2、高阶组件用过吗? 怎么使用的?
    3、继承和组合有什么区别?
    4、React.Component 里面有啥东西?
    5、React Hooks 相关问题.
    6、ref 用过吗?
    7、createPortal 用过吗?
    8、component vs pureCompont 什么区别?
    9、setState 做什么事情?
    
3、Javascript 基础知识?
    1、闭包是什么?
    2、什么是原型链？
    3、浏览器渲染的原理是什么?
    4、安全相关的问题?
    5、this 是什么?
    6、原型链相关的问题?
    7、es5 怎么实现继承?
    8、http 协议相关问题?
    9、url 到加载发生了什么?
    10、Jsonp 的原理是什么?
    11、asyc await 区别?

4、React 原理还要看看.
    1、Fiber 算法?
    2、diff 原理?
    3、调度的原理?
    4、SetState 原理?
    5、react 16 有哪些新的变化?
    6、Redux 中间件?
    7、路由的基本原理是什么?
    8、Immutable.js 原理?

6、数据管理框架方面的问题?
    1、中间件?
    2、数据管理框架的基本流程?
    3、dva vs redux vs mobx 之间的区别?

8、WebPack 方面的问题?
    1、基础用法.
        多页打包怎么做?
    2、打包的原理是什么?
        初始化option.
        开始编译
        从entry 开始递归的分析依赖,对每个依赖模块进行build.
        对模块位置进行解析.
        开始构建某个模块.
        将loader 加载完成的module 进行编译,生成AST树.
        遍历AST，当遇到require 等一些调用表达式时,收集依赖.
        所有的依赖build完,开始优化.
        输出到dist目录.
    3、热加载的原理是什么?
        Webpack Compile: 将 JS 编译成 Bundle
        HMR Server: 将热 新的 件输出给 HMR Rumtime
        Bundle server: 提供 件在浏览 的访问
        HMR Rumtime: 会被注 到浏览 ,  新 件的变化
        bundle.js 构建输出.
        启动阶段:
        编译，打包，传输给bundleServer.js

        文件更新阶段:
        文件系统的变化，编译之后发送给hrmserver ,通知 hmr runtime. 更新代码.

    6、如何做CI/CD?

    8、如何手动写一个webpack/loader/plugins.
    9、如何优化构建的速度.

    10、前端工程化
        

5、小程序相关的问题?
    1、小程序的基本架构?
    2、小程序的生命周期?
    3、小程序文件结构?
    4、小程序的性能优化?
    5、小程序的组件化？

7、NodeJs 方向的问题?
    1、node 的优缺点是什么?
    2、node 基本语法?
    3、node 脚手架.

9、项目中遇到的难点问题?
    1、自适应方案的设计?
    2、ssr 路由如何挂载数据?
    3、数据脱水和注水?
    
10、前端架构方面的问题
    1、
    2、
    3、

11、团队建设方面的问题.
    1、
    2、
    3、
    4、

