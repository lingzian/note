`Vue中DOM的异步更新策略以及nextTick机制`
1、简单来说，Vue在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。
2、观察到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个watcher被多次触发，只会被推入到队列中一次。（
    当数据变化的时候，数据更改会触发 setter 函数，然后通知watcher 进行更新，但是执行watcher的update方法的时候并不会直接更新，而是加入到了一个队列，相同watcher触发多次，只会被推入到队列一次。直到同一事件循环的watcher都被放到了队列里面，然后队列里面的更新节点操作都会被放到nexttick函数里面，然后nextTick会根据运行环境情况用一个微任务或者宏任务事件包起来，然后会在下一个事件循环的Tick中来触发视图更新
）
3、简而言之，就是在一个事件循环中发生的所有数据改变都会在下一个事件循环的Tick中来触发视图更新，这也是一个“批处理”的过程。（注意下一个事件循环的Tick有可能是在当前的Tick微任务执行阶段执行，也可能是在下一个Tick执行，主要取决于nextTick函数到底是使用Promise/MutationObserver还是setTimeout）
# 队列里面的更新节点操作都会被放到nexttick函数里面，然后nextTick会根据运行环境情况用一个微任务或者宏任务事件包起来，然后会在下一个事件循环的Tick中来触发视图更新


`什么是watcher`
每个组件实例会有相应的 watcher 实例,会在组件渲染的过程中记录依赖的所有数据属性（进行依赖收集,还有 computed watcher,user watcher 实例）,之后依赖项被改动时,setter 方法会通知依赖与此 data 的 watcher 实例重新计算（派发更新）,从而使它关联的组件重新渲染。


`响应式原理`
defineProperty 实现的数据劫持，getter 收集依赖，setter 调用更新回调.
每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中解析每个指令或者模板语法（三种watcher,data,watch,computed），生成更新函数绑定在watcher上，总的来说就是把“接触”过的数据记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。


`请你阐述一下对vue虚拟dom的理解`
- 虚拟dom本质上就是一个对dom描述的普通的JS对象在vue中，每个组件都有一个render函数，每个render函数都会返回一个虚拟dom树，这也就意味着每个组件都对应一棵虚拟DOM树

- 为什么需要虚拟dom？
在更新量大的时候直接操作dom带来大量的性能损耗，从而就会极大的降低渲染效率，用虚拟节点的话尽量减少dom操作会比较快

- 虚拟dom是如何转换为真实dom的？（diff算法步骤）
每个组件在编译的时候把模板转换为渲染函数，然后根据dom生成虚拟节点，然后再根据虚拟节点生成真实dom
在数据更新的时候也会重新调用render函数创建虚拟dom树，用新旧虚拟dom树比较，vue会找到最小更新量，然后更新必要的虚拟dom节点，最后修改对应的真实dom。



`Proxy与Object.defineProperty的优劣对比?`
Proxy可以直接监听对象而非属性
Proxy可以直接监听数组的变化
Proxy有多达13种拦截方法,不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的
Proxy返回的是一个新对象,我们可以只操作新的对象达到目的,而Object.defineProperty只能遍历对象属性直接修改