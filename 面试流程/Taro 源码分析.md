https://cloud.tencent.com/edu/learning/course-100005-1349
https://www.imooc.com/article/39381

面试可能会问的问题?

Taro 主要做了两个方面的工作：编译以及运行时适配。编译过程会做很多工作，例如：将 JSX 转换成小程序 .wxml 模板，生成小程序的配置文件、页面及组件的代码等等。编译生成好的代码仍然不能直接运行在小程序环境里.

1、Taro 的设计原理是什么?
在 Taro 中采用的是编译原理的思想，所谓编译原理，就是一个对输入的源代码进行语法分析，语法树构建，随后对语法树进行转换操作再解析生成目标代码的过程.

2、taro 的代码是如何转换成小程序代码的?

taro 框架可以分为运行时和编译时。运行时负责把编译后到代码运行在本不能运行的对应环境中。小程序新建一个页面是使用 Page 方法传入一个字面量对象，并不支持使用类。如果全部依赖编译时的话，那么我们要做到事情大概就是把类转化成对象，把 state 变为 data，把生命周期例如 componentDidMount 转化成 onReady，把事件由可能的类函数（Class method）和类属性函数(Class property function) 转化成字面量对象方法（Object property function).
编译时: 就是一个对输入的源代码进行语法分析，语法树构建，随后对语法树进行转换操作再解析生成目标代码的过程。

how: 通过 Babel 把代码进行转换.

运行时: 编译生成好的代码仍然不能直接运行在小程序环境里,Taro 小程序运行时要配合编译过程，抹平了状态、事件绑定以及生命周期的差异，使得 Taro 组件运行在小程序环境中.

how: 
注册程序、页面以及自定义组件.
将组件的 state 转换成小程序组件配置对象的 data.

将组件的生命周期对应到小程序组件的生命周期.
```
1、attached => componentWillMount.
2、ready => componentDidMount.
3、detached => componentWillUnmount.
```

将组件的事件处理函数对应到小程序的事件处理函数.

cli 工具链怎么写?

============================================

运行时:
Component(createComponent(customComponent)).

createComponent 方法主要做了这样几件事情：
将组件的 state 转换成小程序组件配置对象的 data

将组件的生命周期对应到小程序组件的生命周期.
1、attached => componentWillMount.
2、ready => componentDidMount.
3、detached => componentWillUnmount.

将组件的事件处理函数对应到小程序的事件处理函数.

taro 转成 h5 的路由是怎么回事?
原理是: 模仿了react-router 的方案，监听页面路径变化,再触发 UI 更新。这是React的精髓之一，单向数据流。
@tarojs/router包中包含了一个轻量的history实现。history中维护了一个栈，用来记录页面历史的变化。对历史记录的监听，依赖两个事件：hashchange和popstate.


4、Taro 有哪些限制?
* 不能使用 Array#map 之外的方法操作 JSX 数组.
Taro 在小程序端实际上把 JSX 转换成了字符串模板，而一个原生 JSX 表达式实际上是一个 React/Nerv 元素(react-element)的构造器，因此在原生 JSX 中你可以随意地对一组 React 元素进行操作。但在 Taro 中你只能使用 map 方法，Taro 转换成小程序中 wx:for

* 暂不支持在 render() 之外的方法定义 JSX
由于微信小程序的 template 不能动态传值和传入函数，Taro 暂时也没办法支持在类方法中定义 JSX。

* 不能在 JSX 参数中使用对象展开符.
微信小程序组件要求每一个传入组件的参数都必须预先设定好，而对象展开符则是动态传入不固定数量的参数。所以 Taro 没有办法支持该功能。

* 不支持无状态组件.
由于微信的 template 能力有限，不支持动态传值和函数，Taro 暂时只支持一个文件只定义一个组件。为了避免开发者疑惑，暂时不支持定义 stateless component.

* 组件传递函数属性名以 on 开头.
在 Taro 中，父组件要往子组件传递函数，属性名必须以 on 开头,微信小程序端组件化是不能直接传递函数类型给子组件的，在 Taro 中是借助组件的事件机制来实现这一特性，而小程序中传入事件的时候属性名写法为 bindmyevent 或者 bind:myevent.


--------------------------------------------
5、Taro 的基本原理是什么?

taro 是怎么支持redux 的?
1、redux 基本原理是什么?

2、Taro 设计思想及架构.
核心思想:
在 Taro 中采用的是编译原理的思想，所谓编译原理，就是一个对输入的源代码进行语法分析，语法树构建，随后对语法树进行转换操作再解析生成目标代码的过程。

抹平多端差异:
基于编译原理，我们已经可以将 Taro 源码编译成不同端上可以运行的代码了，但是这对于实现多端开发还是远远不够。因为不同的平台都有自己的特性，每一个平台都不尽相同，这些差异主要体现在不同的组件标准与不同的 API 标准以及不同的运行机制上.

Taro 采用了定制一套运行时标准来抹平不同平台之间的差异:这一套标准主要以三个部分组成，包括标准运行时框架、标准基础组件库、标准端能力 API，其中运行时框架和 API 对应 @taro/taro，组件库对应 @tarojs/components，通过在不同端实现这些标准，从而达到去差异化的目的.

而在标准的定制上，起初我们想重新定制一套标准规范，但是发现在所有端都得实现这套标准的成本太高，所以我们就思考为什么不以一个端的组件库、API 为标准呢？这样不仅省去了标准定制的时间，而且在这个端上我们可以不用去实现这套标准了。最终在所有端中，我们挑选了微信小程序的组件库和 API 来作为 Taro 的运行时标准，因为微信小程序的文档非常完善，而且组件与 API 也是非常丰富，同时最重要的是，百度小程序以及支付宝小程序都是遵循的微信小程序的标准，这样一来，Taro 在实现这两个平台的转换上成本就大大降低了.

编译时.  taro-cli 编译.

运行时.  taro-component,taro Api.

多端应用.

CLI 原理及不同端的运行机制:
taro-cli 负责 Taro 脚手架初始化和项目构建的的命令行工具，NPM 包的链接在这里.

包管理与发布:
taro-cli
    src.  // taro build 命令，根据type 调用不同的脚本.
    templates // 脚手架模板.
    bin  // 命令行


taro init.
生成项目.
用到的核心库: nodejs.


模版文件操作. 
拷贝模板文件.
更新已经存在的文件内容.

taro build.
taro build 命令是整个 Taro 项目的灵魂和核心，主要负责多端代码编译.

Taro 命令的关联，参数解析等和 taro init 其实是一模一样的，那么最关键的代码转换部分是怎样实现的呢。

编译工作流与抽象语法树（AST）.
```
首先是 Parse，将代码解析（Parse）成抽象语法树（Abstract Syntex Tree），然后对 AST 进行遍历（traverse）和替换(replace)（这对于前端来说其实并不陌生，可以类比 DOM 树的操作），最后是生成（generate），根据新的 AST 生成编译后的代码.

Babel 编译器.

解析页面 Config 配置


Taro Update.

```

### Taro 组件库及 API 的设计与适配.
组件差异:

API 差异:
定制化、接口不一、能力限制.

### 设计思路
由于组件和 API 定制程度的不同，相同功能的组件和 API 提供的能力不完全相同，在设计的时候，对于端差异较小的不影响主要功能的，我们直接使用相应端对应的组件 API 来实现，并申明特性的支持程度，对于端差异较大的且影响了主要功能的，则通过封装的形式来完成，并申明特性的支持程度，绝大部分的组件 API 都是通过这种形式来实现的.

### JSX 转换微信小程序模板的实现.

```
在一个优秀且严格的规范限制下，从更高抽象的视角（语法树）来看，每个人写的代码都差不多。
也就是说，对于微信小程序这样不开放不开源的端，我们可以先把 React 代码想象成一颗抽象语法树，根据这颗树生成小程序支持的模板代码，再做一个小程序运行时框架处理事件和生命周期与之兼容，然后把业务代码跑在运行时框架就完成了小程序端的适配。
```

这里其实我们是将 React 的 API 和 JSX 语法当成一种 DSL（Domain-specific language），只要将源代码编译成个各平台的对应语法就能达到跨平台的目的。而微信小程序和 React 的 API 和 JSX 语法差距巨大，Taro 是怎么编译的呢？这就要从代码是什么这个问题开始讲起.

### 代码的本质
不管是任意语言的代码，其实它们都有两个共同点：
它们都是由字符串构成的文本
它们都要遵循自己的语言规范

react api 代码通过 Babel,Parse 进行转义.

 Taro 是如何将一个 JSX 文件转换成 JavaScript 文件、CSS 文件以及 JSON 文件?

 Taro 的结构主要分两个方面：运行时和编译时。运行时负责把编译后到代码运行在本不能运行的对应环境中，你可以把 Taro 运行时理解为前端开发当中 polyfill。举例来说，小程序新建一个页面是使用 Page 方法传入一个字面量对象，并不支持使用类。如果全部依赖编译时的话，那么我们要做到事情大概就是把类转化成对象，把 state 变为 data，把生命周期例如 componentDidMount 转化成 onReady，把事件由可能的类函数（Class method）和类属性函数(Class property function) 转化成字面量对象方法（Object property function）等等。

但这显然会让我们的编译时工作变得非常繁重，在一个类异常复杂时出错的概率也会变高。但我们有更好的办法：实现一个 createPage 方法，接受一个类作为参数，返回一个小程序 Page 方法所需要的字面量对象。这样不仅简化了编译时的工作，我们还可以在 createPage 对编译时产出的类做各种操作和优化。通过运行时把工作分离了之后，再编译时我们只需要在文件底部加上一行代码 Page(createPage(componentName)) 即可.

文件是怎么转义的:
```
import Taro, { Component } from '@tarojs/taro'
import { View, Text } from '@tarojs/components'

class Home extends Component {
  config = {
    navigationBarTitleText: '首页'
  }
  state = {
    numbers: [1, 2, 3, 4, 5]
  }
  handleClick = () => {
    this.props.onTest()
  }
  render () {
    const oddNumbers = this.state.numbers.filter(number => number & 2)
    return (
      <ScrollView className='home' scrollTop={false}>
        奇数：
        {
          oddNumbers.map(number => <Text onClick={this.handleClick}>{number}</Text>)
        }
        偶数：
        {
          numbers.map(number => number % 2 === 0 && <Text onClick={this.handleClick}>{number}</Text>)
        }
      </ScrollView>
    )
  }
}
```

### 运行时揭秘 - 小程序运行时.
为了使 Taro 组件转换成小程序组件并运行在小程序环境下， Taro 主要做了两个方面的工作：编译以及运行时适配。编译过程会做很多工作，例如：将 JSX 转换成小程序 .wxml 模板，生成小程序的配置文件、页面及组件的代码等等。编译生成好的代码仍然不能直接运行在小程序环境里，那运行时又是如何与之协同工作的呢？

```
Component(createComponent(customComponent))
```
createComponent 方法主要做了这样几件事情：
将组件的 state 转换成小程序组件配置对象的 data.
将组件的生命周期对应到小程序组件的生命周期.
将组件的事件处理函数对应到小程序的事件处理函数.

总结:
Taro解决问题的两个方式非常熟悉了，无非就是编译时与运行时.












我们需要了解 taro-cli 包与 Taro 工程的关系.


扩展:
Lerna 是一个用来优化托管在 Git/NPM 上的多 package 代码库的工作流的一个管理工具，可以让你在主项目下管理多个子项目，从而解决了多个包互相依赖，且发布时需要手动维护多个包的问题.





react 中只定义了基本的api,具体实现不在 react 里面,而是在具体的对应的包里面,react-native,react-art,react-dom,react-server.
taro 的设计思路其实有点类似.




taro 组成:

Taro 运行时框架.
@tarojs/taro     Taro 运行时框架.
@tarojs/taro-h5  Taro H5 运行时框架.
@tarojs/taro-rn  Taro React Native 运行时框架

```
taro  多端解决方案基础框架
    src
        index.js  定义api 的方法.

├── dist                                        
├── taro                                       
│   ├── components                              
│   │   ├── button                              
│   │   │   ├── index.js                        
│   │   ├── checkbox                              
│   │   │   ├── index.js                       
```
taro 规范:
1、



3、




写一篇文章进行比较:

taro vs remax

setState 做了什么?
从 react 0.14 版本之后，引擎代码就从 react 包中抽离了，react 包仅仅做通用接口抽象.
也就是说，react 包定义了标准的状态驱动模型的 API，而 react-dom react-native react-art 这些包是在各自平台的具体实现.
各平台具体的渲染引擎实现被称为 reconciler, 通过这个链接可以看到 react-dom react-native react-art 这三个包的 reconciler 实现.
这说明了 react 包仅告诉你 React 拥有哪些语法，而并不关心如何实现他们，所以我们需要结合 react 包与 react-xxx 一起使用.

setState 怎么调用平台实现?
每个平台对 UI 更新逻辑的实现，会封装在 updater 函数里，所以不同平台代码会为组件添加各自的 updater 实现.


接口的力量:
在日常编程中，接口也拥有的强大力量，下面举几个例子.
UI 组件跨三端的接口:
由于 RN、Weex、Flux 的某些不足，越来越多的人选择 “一个思想三端实现” 的方式做跨三端的 UI 组件，这样既兼顾了性能，又可以照顾到平台差异性，对不同平台组件细节做定制优化,要实施这个方案，最大问题就是接口约定。一定要保证三套实现遵循同一套 API 接口，业务代码才可以实现 “针对任意一个平台编写，自动移植到其他平台”.

小程序融合的方案:
现在这种方案很火。通过基于 template 或者 jsx 的语法，一键发布到各平台小程序应用.
这种方案一定会抽象一套通用语法，甚至几乎等价与 react 与 react-dom 的关系：所有符合规范的语法，转化为各小程序平台的实现

总结:
react 中只定义了基本的api,具体实现不在 react 里面,而是在具体的对应的包里面,react-native,react-art,react-dom,react-server.
taro 的设计思路其实有点类似.


taro 组成:

Taro 运行时框架.
@tarojs/taro     Taro 运行时框架.
@tarojs/taro-h5  Taro H5 运行时框架.
@tarojs/taro-rn  Taro React Native 运行时框架

```
taro  多端解决方案基础框架
    src
        index.js  定义api 的方法.

├── dist                                        
├── taro                                       
│   ├── components                              
│   │   ├── button                              
│   │   │   ├── index.js                        
│   │   ├── checkbox                              
│   │   │   ├── index.js                       
```
taro 规范:
1、

生命周期的对应:
componentWillMount(),  对应: onLaunch().
监听程序初始化，初始化完成时触发(全局只触发一次)
在此生命周期中通过 this.$router.params，可以访问到程序初始化参数.
componentDidShow()  对应:onShow().
componentDidHide()  对应:onError().



参考文献:
* http://taro-docs.jd.com/taro/docs/tutorial.html

taro 自身的限制:
1、不能使用 Array#map 之外的方法操作 JSX 数组.

```
Taro 在小程序端实际上把 JSX 转换成了字符串模板，而一个原生 JSX 表达式实际上是一个 React/Nerv 元素(react-element)的构造器，因此在原生 JSX 中你可以随意地对一组 React 元素进行操作。但在 Taro 中你只能使用 map 方法，Taro 转换成小程序中 wx:for
```
2、暂不支持在 render() 之外的方法定义 JSX
由于微信小程序的 template 不能动态传值和传入函数，Taro 暂时也没办法支持在类方法中定义 JSX。

3、不能在 JSX 参数中使用对象展开符.
微信小程序组件要求每一个传入组件的参数都必须预先设定好，而对象展开符则是动态传入不固定数量的参数。所以 Taro 没有办法支持该功能

4、不支持无状态组件.
由于微信的 template 能力有限，不支持动态传值和函数，Taro 暂时只支持一个文件只定义一个组件。为了避免开发者疑惑，暂时不支持定义 stateless component.

5、组件传递函数属性名以 on 开头.
在 Taro 中，父组件要往子组件传递函数，属性名必须以 on 开头,微信小程序端组件化是不能直接传递函数类型给子组件的，在 Taro 中是借助组件的事件机制来实现这一特性，而小程序中传入事件的时候属性名写法为 bindmyevent 或者 bind:myevent.

组件化:
微信小程序的自定义组件样式默认是不能受外部样式影响的，例如在页面中引用了一个自定义组件，在页面样式中直接写自定义组件元素的样式是无法生效的。这一点，在 Taro 中也是一样，而这也是与大家认知的传统 Web 开发不太一样.

render props
render props 是基于小程序 slot 机制实现的。 因此它受到的限制和 children 与组合的限制一样多.

语法特性:

taro vs react 区别:
这是 Taro 和 React 另一个不同的地方：React 的 setState 不一定总是异步的，他内部有一套事务机制控制，且 React 15/16 的实现也各不相同。而对于 Taro 而言，setState 之后，你提供的对象会被加入一个数组，然后在执行下一个 eventloop 的时候合并它们.

多端开发:

taro 做得比较好的地方:
1、社区共享。
2、Taro 物料市场.
3、Taro 交流社区.

