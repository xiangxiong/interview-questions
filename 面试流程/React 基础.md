
* 如何在前面10 min 钟充分发挥你的优势?

> 虚拟dom 的原理是什么？

   1、用Javascript 对象结构表示DOM树的结构; 然后用这个树构建一个真正的DOM树,插入到文档当中。

   2、当状态变更的时候，重新构造一颗新的对象树。然后用新的树和旧的树进行比较，记录两颗树的差异。

   3、把2所记录的差异应用到步骤1所构建的真正的DOM树上,视图就更新了.

   详解diff:
    React 将 Virtual DOM 树转换成 actual dom 树的最少操作过程叫调和(reconciliation).
    将 O(n3) 复杂度的问题转换成O(n) 复杂度的问题.

   1、React 15 中,每次更新时, 都是从根组件或setState 后组件开始, 更新整个子树.

   2、React 16 早期将原来的diff 过程拆分成两个阶段, 第一个阶段叫 reconcile, 也就是原来diff 虚拟dom 的过程。第二个阶段叫commit,commit 就是对应早期版本的patch过程.，在并发模式下，可能多次reconcile 才执行一次，commit.

   第一阶段:将虚拟DOM转换成Fiber,Fiber 转换成组件实例或真实DOM（不插入DOM树,插入DOM树会reflow）,还会执行一些轻量的钩子(ComponentWillMount) 等.Fiber转换后两者明显会耗时.

   第二阶段:commit 是将水面下的效果浮现出来,比如将节点插入到Dom 树, 修复节点的属性样式文本内容, 执行如componentDidXXX 这样的重量钩子,执行ref 操作.

> setState 更新的流程是什么?

   1、setState 的调用栈流程,  this.setState 之后会让 newState 进入到更新队列, 然后在调用 enqueueUpdate 会判断是否处于批量更新的模式，如果是组件保存到dirtyComponets 里面，如果不是 会遍历 dirtyComponets 调用 updateComponent 更新 pending state or
   props.

   ```
    function unbatchedUpdates<A, R>(fn: (a: A) => R, a: A): R {
    if (isBatchingUpdates && !isUnbatchingUpdates) {
        isUnbatchingUpdates = true;
        try {
        return fn(a);
        } finally {
            isUnbatchingUpdates = false;
        }
    }
    return fn(a);
    }
   ```

   2、batchedUpdates 方法中涉及到事务的批量处理.

    事务就是将需要执行的方法使用wrapper 封装起来，在通过事务提供的 perform 方法执行。 而在perform 之前, 先执行所有 wrapper 中的initialize 方法,执行完 peform 之后，在执行所有的close 方法。

   3、setState 是同步的还是异步的?

   官方的解释:
    (1)、保持内部的一致性.
    假设state 是同步的,state的更新的值可以立即获取,如果这时候要将状态提升到父组件，以供多个兄弟组件共享，但是this.props 的值并不能可以。而且在没有重新渲染父组件的情况下, 我们不能立即更新 this.props. 如果要立即更新this.props。就必须放弃批处理.
    所以为了解决这样的问题, 在React 中 this.state 和 this.props 都是异步更新的。react 模型更愿意保证内部的一致性和状态提升的安全性.

    (2)、性能优化.
    如果是同步更新的话，react 就没办法做state 的合并更新.

    (1)、setState 只在合成事件和钩子函数中是“异步”的，在原生事件和 setTimeout 中都是同步的。
    (2)、setState的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。
    (3)、setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次 setState ， setState 的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时 setState 多个不同的值，在更新时会对其进行合并批量更新.

    https://github.com/olifer655/react/issues/12

    ```
    Component.prototype.forceUpdate = function(callback) {
    this.updater.enqueueForceUpdate(this, callback, 'forceUpdate');
    };
    https://www.zhihu.com/search?type=content&q=setState

    理解跨平台的方案的实现方式.
    ```
    ----- 
    setState 本身的方法调用是同步的,但是调用了setState 方法并不标志着立马就更新了,这个更新时要根据当前的执行环境的上下文来判断的,如果是处于批量更新的情况下，我的状态不是立马更新的，而我不处于批量更新的情况下，因为他就有可能是立马更新的。因为我们处于async model 和 currentModel 这种异步渲染的情况下，也不是立马就更新的.  因为进入异步渲染会进入异步更新调度的过程，所以不会里面更新.

    (3)、那什么场景下是异步的，可不可能是同步，什么场景下又是同步的?
    使用 settimeout 方法可以将异步变成同步方法.
    为什么: 因为设置成settimeout 的时候，执行上下文环境发生了变化，变成了window 了,
    isBatchingUpdates 就始终为false. 所以会在 requestWork 里面执行 performSyncWork()
    方法。 会让应用的性能变得很低.

    * 
    会等到所有的state 全部更新完之后，才会触发更新.
    ```
        function requestWork(root, expirationTime) {
        addRootToSchedule(root, expirationTime);
        if (isRendering) {
            // Prevent reentrancy. Remaining work will be scheduled at the end of
            // the currently rendering batch.
            return;
        }
        if (isBatchingUpdates) {
            // Flush work at the end of the batch.
            if (isUnbatchingUpdates) {
            // ...unless we're inside unbatchedUpdates, in which case we should
            // flush it now.
            nextFlushedRoot = root;
            nextFlushedExpirationTime = Sync;
            performWorkOnRoot(root, Sync, true);
            }
            return;
        } // TODO: Get rid of Sync and use current time?
        if (expirationTime === Sync) {
            performSyncWork();
        } else {
            scheduleCallbackWithExpirationTime(root, expirationTime);
        }
        }

    function batchedUpdates$1(fn, a) {
        var previousIsBatchingUpdates = isBatchingUpdates;
        isBatchingUpdates = true;

        try {
            return fn(a);
        } finally {
            isBatchingUpdates = previousIsBatchingUpdates;

            if (!isBatchingUpdates && !isRendering) {
            performSyncWork();
            }
        }
        }
    ```
    batchedUpdates 会让所有的state 全部更新完之后,在执行finally 方法 performSyncWork（） 去更新状态.

> 生命周期方面相关问题.

* 基本原理:
   * 生命周期的本质是状态机,
   * 三个阶段管理: MOUTING,RECEIVE_PROPS,UNMOUNTING. 负责通知组件当前处于哪个阶段，应该指向哪些生命周期.


* MOUTING 对应的方法:

    mountComponent 负责管理生命周期中的 getInitialState、componentWillMount、render 和 componentDidMount.

* RECEIVE_PROPS 对应的方法:

    updateComponent 负责管理生命周期中的 componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、render 和 componentDidUpdate.

* UNMOUNTING 对应的方法:

    unmountComponent 负责管理生命周期中的 componentWillUnmount。


* 出彩的地方:
    * 16 版本里面新增了:componentDidCatch 这个钩子函数，划分出错误组件与边界组件，每个边界组件能修复下方组件错误一次，再次出错，转交给更上层的组件来处理，解决异常处理问题.
    * 自React16起, 任何未被错误边界捕获的错误将会导致整个React 组件被卸载. 
    * 注意错误边界仅可以捕获其子组件的错误，它无法捕获其自身的错误。如果一个错误边界无法渲染错误信息，则错误会冒泡至最近的上层错误边界，这也类似于 JavaScript 中 catch {} 的工作机制.

    * https://codesandbox.io/s/lOo5AV12M.
    * https://codepen.io/gaearon/pen/wqvxGa?editors=0010

 * 在React Hook 中已经没有生命周期的概念, 取而代之用 useEffect 去模拟对应的生命周期方法.
* 概念性的问题:
* react 生命周期有哪些?
    * react 组件的生命周期有三个不同的阶段:
    - 初始化阶段: 这是组件即将开始其生命并进入DOM阶段.
    - 更新阶段: 一旦组件被添加到DOM，它只有在prop 或状态发生变化时才可能更新和重新渲染。这些只发生在这个阶段.
    - 卸载阶段: 这是组件生命周期的最后阶段,组件被销毁从DOM中删除.

* 一些重要的生命周期方法:
    * componentWillMount**()** – 在渲染之前执行，在客户端和服务器端都会执行。
    * componentDidMount – 仅在第一次渲染后在客户端执行.
    * componentWillReceiveProps 当从父类接收到 props 并且在调用另一个渲染器之前调用.
    * shouldComponentUpdate – 根据特定条件返回 true 或 false。如果你希望更新组件.
    * componentWillUpdate**()** – 在 DOM 中进行渲染之前调用.
    * componentDidUpdate**()** – 在渲染发生后立即调用.
    * componentWillUnmount**()** – 从 DOM 卸载组件后调用。用于清理内存空间.

> React Hooks 解决了什么问题?

hooks 基本原理:

* React Hooks 相关问题:

* hooks 解决了什么问题?
    * 1. 在组件之间复用状态逻辑很难
            * render props,hoc 这类方案需要重新组织你的组件结构，使得代码难以理解，高级组件和render props 这些抽象层的组件会形成地狱嵌套。 React 需要为共享状态逻辑提供更好的原生途径
            * Hook 使你在无需修改组件结构的情况下复用状态逻辑。
            * 可以使用 Hook 从组件中提取状态逻辑，使得这些逻辑可以单独测试并复用。

    * 2. 复杂组件变得难以理解
            * Hook 将组件中相互关联的部分拆分成更小的函数.
            * 我们经常维护一些组件，组件起初很简单，但是逐渐会被状态逻辑和副作用充斥。每个生命周期常常包含一些不相关的逻辑.
            * 组件常常在 componentDidMount 和 componentDidUpdate 中获取数据。但是，同一个 componentDidMount 中可能也包含很多其它的逻辑，如设置事件监听，而之后需在 componentWillUnmount 中清除.
            * 而完全不相关的代码却在同一个方法中组合在一起。如此很容易产生 bug，并且导致逻辑不一致.

    * 3. 使用 Hook 其中一个目的就是要解决 class 中生命周期函数经常包含不相关的逻辑，但又把相关逻辑分离到了几个不同方法中的问题.

* hooks 基本原理.
    * 初次渲染的时候，按照 useState，useEffect 的顺序，把 state，deps 等按顺序塞到 memoizedState 数组中。
    * 更新的时候，按照顺序，从 memoizedState 中把上次记录的值拿出来。


* hooks 的api 的用法.

        * useState.
            * State 变量可以很好地存储对象和数组，因此，你仍然可以将相关数据分为一组。然而，不像 class 中的 this.setState，更新 state 变量总是替换它而不是合并它。

        * useEffect.
            * 副作用.

        * useContext.
            * 创建对象用的.

        * useReducer.
            * 可以用来做负载逻辑的数据管理.

        * useCallback.
            * Hook 允许你在重新渲染之间保持对相同的回调引用以使得 shouldComponentUpdate 继续工作.

        * useMemo.
            * memo  如果 props 的数据没有变化不会触发重新渲染..
            * React.memo 等效于 PureComponent，但它只比较 props.
            * React.memo 不比较 state，因为没有单一的 state 对象可供比较.

        * useRef.

        * useImperativeHandle.

        * useLayoutEffect.
            * 大部分情况下，使用 useEffect 就可以帮我们处理组件的副作用，但是如果想要同步调用一些副作用，比如对 DOM 的操作，就需要使用 useLayoutEffect，useLayoutEffect 中的副作用会在 DOM 更新之后同步执行.

        * useDebugValue.

    为什么只能在函数最外层调用 Hook，不要在循环、条件判断或者子函数中调用？

    为什么 useEffect 第二个参数是空数组，就相当于 ComponentDidMount ，只会执行一次？

    自定义的 Hook 是如何影响使用它的函数组件的？

    Capture Value 特性是如何产生的？
        * 每一次 ReRender 的时候，都是重新去执行函数组件了，对于之前已经执行过的函数组件，并不会做任何操作.

    参考文献:
    * https://juejin.im/post/5ce53c636fb9a07ef06f6b2b.

    * 2、核心知识点有哪些?
        * 第三方的库会有哪些改变. redux,react router.
        * 如何测试react hook 组件.
        * 生命周期如何对应hook?
            * useEffect 将 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API.

        * 调用 useState 方法的时候做了什么?
            * 

        * 如何获取数据?
        *  https://codesandbox.io/s/jvvkoo8pq3.
        *  https://www.robinwieruch.de/react-hooks-fetch-data/.


        * 相关的约束:
            * 1、只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。
            * 2、只能在 React 的函数组件中调用 Hook。

             * hooks faq
            * https://zh-hans.reactjs.org/docs/hooks-faq.html.
            * https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889

    * 4、什么是hook?
            * Hook 是一些可以让你在函数组件里面使用state 以及生命周期等特性的函数。Hook 不能在class 中使用。

    * 3、如何对现有代码进行改善?
        * 
        * 
        * 

    * 4、我将如何使用?
        * 写一个具体的例子.
        * 

    * 和 class 相比有哪些优点:
        * 1、在这个 class 中，我们需要在两个生命周期函数中编写重复的代码。
        * useEffect 做了什么？
            * 通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作，并且在执行 DOM 更新之后调用它.
        * 为什么在组件内部调用 useEffect？
        * useEffect 会在每次渲染后都执行吗?
            * 是的，默认情况下，它在第一次渲染之后和每次更新之后都会执行.
        * 忘记正确地处理 componentDidUpdate 是 React 应用中常见的 bug 来源.
            * 1、举例子:好友在线状态的例子. 根据好友id 显示是否在线的状态，如果状态更新了的话，卸载的时候还可能是记住了原来的id。
            * 2、hook 并不需要特定的代码来处理更新逻辑，因为 useEffect 默认就会处理。它会在调用一个新的 effect 之前对前一个 effect 进行清理。

        * 跳过 Effect 进行性能优化
            * 每次渲染后都执行清理或者执行 effect 可能会导致性能问题.
                * 这是很常见的需求，所以它被内置到了 useEffect 的 Hook API 中。如果某些特定值在两次重渲染之间没有发生变化，你可以通知 React 跳过对 effect 的调用，只要传递数组作为 useEffect 的第二个可选参数即可
                ```
                useEffect(() => {
                document.title = `You clicked ${count} times`;
                }, [count]); // 仅在 count 更改时更新
                ```
                * 如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）作为第二个参数。这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。这并不属于特殊情况 —— 它依然遵循依赖数组的工作方式.

    * 注意:
        * 与 class 组件中的 setState 方法不同，useState 不会自动合并更新对象。你可以用函数式的 setState 结合展开运算符来达到合并更新对象的效果.
        * 要记住 effect 外部的函数使用了哪些 props 和 state 很难。这也是为什么 通常你会想要在 effect 内部 去声明它所需要的函数。 这样就能容易的看出那个 effect 依赖了组件作用域中的哪些值.
        
    * 思考题目:
         * 问题:Hook 使用了 JavaScript 的闭包机制 是怎么使用的?
         * react hooks 如何在项目中使用? 一个新的项目. 写5个页面. ok
         * useReducer 之后 还要 redux 干啥？ ok了.

    * 好的项目:
        * https://github.com/tannerlinsley/react-form.
        * https://github.com/kentcdodds/advanced-react-hooks | https://kentcdodds.com/workshops/advanced-react-hooks/
        * https://juejin.im/post/5bf20ce6e51d454a324dd0e6#heading-4  hooks 的一些使用场景.

> React 常用 API.

    * CreatePortal

        * 解决什么问题:
        Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案.

        * 使用了 ReactDOM.unstable_renderSubtreeIntoContainer 就是更新组件传入到Dom 节点上. 这就实现了跨组件的DOM 操作.

        ```
            // 在 DOM 中有两个容器是兄弟级（siblings）
            const appRoot = document.getElementById('app-root');
            const modalRoot = document.getElementById('modal-root')

            modalRoot.appendChild(this.el);

            render() {
                return ReactDOM.createPortal(
                this.props.children,
                this.el,
                );
            }
        ```
        之前是怎么做的:
        通过 unstable_renderSubtreeIntoContainer 这个api 来实现渲染的.
        1、它什么都不给自己画，render返回一个null就够了；
        2、它做得事情是通过调用renderPortal把要画的东西画在DOM树上另一个角落。

        缺陷:
        v16之前的React Portal实现方法，有一个小小的缺陷，就是Portal是单向的，内容通过Portal传到另一个出口，在那个出口DOM上发生的事件是不会冒泡传送回进入那一端的.

        参考文献:
        https://zhuanlan.zhihu.com/p/29880992

     * Context api：
        定义: Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法.

        场景: redux 传递store, mobx 传递store.


     * 高阶组件:
        定义: 1、高阶组件本质是一个高阶函数，最大的好处是解耦和灵活性.

        使用场景:
        1、抽取公用代码和业务逻辑.功能和页面类似的页面，可以把一些共同的操作抽离到HOC组件中
        2、抽离state. react 处理表单的时候，如果使用了受控组件，可以把onChange 事件同步改变state 的代码，封装到高阶组件中,还有提交的事件。
        3、劫持渲染.(loading 动效,页面权限管理)。
        4、操作props,增加用户信息属性对象.
        5、反向继承. 场景:两个页面的相似度非常高，要新加一些属性的时候可以考虑使用.

        缺点:
            1、refs不会传递.
            2、多个组件包裹会出现 属性冲突。

     * ref 的用法:
        1、用过, ref用于访问 render 方法中创建 dom 元素或者是 React 组件的实例.
        2、ref 有三种用法, string ref,call back ref,createRef.
        3、推荐使用:createRef.callback ref 不推荐使用 string ref.

        为什么不推荐使用ref?
        1、当 ref 定义为string 时,需要 React 追踪当前正在渲染的组件,在 reconciliation 阶段, React Element 创建.和更新的过程中, ref 会被包装为一个闭包函数, 等待 commit 阶段被执行，这会对React 的性能产生一些影响.
        2、当使用 render callback 模式的时候,使用 string ref 会造成 ref 挂载位置产生歧义.

        React.forwardRef:
        主要用于穿过父元素直接获取子元素的 ref。在提到 forwardRef 的使用场景之前，我们先来回顾一下，HOC（higher-order component）在 ref 使用上的问题，HOC 的 ref 是无法通过 props 进行传递的，因此无法直接获取被包裹组件（WrappedComponent），需要进行中转.

    *key 值有什么用?

      作为 key 的键应该符合以下条件
        唯一的：元素的 key 在它的兄弟元素中应该是唯一的。没有必要拥有全局唯一的 key.
        稳定的：元素的 key 不应随着时间，页面刷新或是元素重新排序而变.
        可预测的：你可以在需要时拿到同样的 key，意思是 key 不应是随机生成的。作为 key 的键应该符合以下条件.

      key 有啥用?  
        减少dom比对次数,提高渲染的性能.

      有 key 和没 key 有啥区别?
        * 而key属性的使用，则涉及到diff算法中同级节点的对比策略，当我们指定key值时，key值会作为当前组件的id，diff算法会根据这个id来进行匹配。如果遍历新的dom结构时，发现组件的id在旧的dom结构中存在，那么react会认为当前组件只是位置发生了变化，因此不会将旧的组件销毁重新创建，只会改变当前组件的位置，然后再检查组件的属性有没有发生变化，然后选择保留或修改当前组件的属性.
        * 如果没有显式指定，react会把当前组件数据源的index作为默认的key值.这时候执行diff算法的时候，发现索引值已经是存在的,并且组件的位置还是原来的位置，所以，直接保留了原组件.但是我们的输入框没有改变.

    * render Props 的用法
        解决什么问题: 解决横切关注点。是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术.
        原理: render CallBack
        优缺点
            优 点: 解决hoc 属性冲突的问题.
            缺 点: 这类方案需要重新组织你的组件结构，这可能会很麻烦，使你的代码难以理解, providers，consumers，高阶组件，render props 等其他抽象层组成的组件会形成“嵌套地狱”.

    * 受控组件:
        * 受控组件:
            * 在一个受控组件中，表单数据是由 React 组件来管理的
        * 非受控组件: 
            * 另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理.
    

    * 组合 vs 继承:

        编写组件的角度来看:
        组合: 有些组件无法提前知晓它们子组件的具体内容。在 Sidebar（侧边栏）和 Dialog（对话框）等展现通用容器（box）的组件中特别容易遇到这种情况. 我们建议这些组件使用一个特殊的 children prop 来将他们的子组件传递到渲染结果中.

        集成: 我们并没有发现需要使用继承来构建组件层次的情况.

        从概念上来看:
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


    * Component vs PureComponent 什么区别?
        1、PureComponent 继承了 Component。但是在原型链式扩展了 isPureComponent 的属性.
        2、有 isPureComponent  的属性会进行浅比较.

        浅比较是怎么实现的?
        1、是比较两个对象是否相等. 源码是通过Object.is 方法来实现的. 比较的是对象是否相等. 但是没有比较多层嵌套的对象以及函数.
        如何实现:
        1、通过判断基本数据类型. Object.is 
        2、判断两个对象的长度是否一致。
        3、然后通过循环的方式判断两个对象是否相等


    * React Fiber 部分:
        * 


    * React 源码:
        *  把 react schedule 的流程图画出来,讲解.

    

