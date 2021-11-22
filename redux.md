redux


Action 把数据从应用传到 store 。它是 store 数据的唯一来源。通过 store.dispatch() 将 action 传到 store。


Reducers 
指定了应用状态的变化如何响应 actions 并发送到 store 的，actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state。
更新state的函数称为reducer，它是一个纯函数，接受旧的state和action，返回新的state。


store
store就是把action和reducer联系到一起的对象，所有state都存储到store上， 提供state dispatch
在一个应用程序中只能有一个store对象。当一个store接收到一个action，它将把这个action代理给相关的reducer。reducer是一个纯函数，它可以查看之前的状态，执行一个action并且返回一个新的状态。

总流程
Action通过Store的Dispatch方法派发给Store, Store接收到Action 连同之前State 一同传给Reducer ，Reducer会返回一个新的数据给Store ，Store然后去改变自己的State，这就是Redux的标准流程


ispatch方法进行封装，这时给Dispatch传入是一个对象，它会直接把这个对象传给Store ，如果Dispatch传入是一个函数的话，先执行 ，然后会根据传入的参数不同进行不同的事情。

总结 在react仲直接使用redux的方式导致react组件和redux的耦合度很高而且通过dispatch，getState等函数调用很麻烦，这时候就应该学习react-redux的使用了，（每次都要引入store）
毕竟该插件包装了redux后专门用于react中。







react-redux

状态管理解决react数据从顶部组件一层层通过props传递下来(层级多不方便， 组件复用的时候也会造成影响)
Redux
存放一个数据对象
外界能访问到这个数据
外界也能修改这个数据
当数据有变化的时候，通知订阅者（重新执行订阅函数，重新渲染ui）

怎么结合到React里面呢？ 我们可以在应用初始化的时候，创建一个window.store = createStore(reducer)，
然后在需要的地方通过store.getState()去获取数据，通过store.dispatch去更新数据，
通过store.subscribe去订阅数据变化然后进行setState...如果很多地方都这样做一遍，实在是不堪其重，而且，还是没有避免掉全局变量的不优雅。


context
react 组件间传递数据是通过 props 向下（也就是想子组件传递），是单向传递的，从父级一层一层地通过 props 地向下传递到子子孙孙，
有的时候我们组件一层一层的嵌套多层，这样这种方式一层一层传递麻烦，我们可不可以进行跃层传递，这就会用到 context。
context可以使子孙组件直接获取父级组件中的数据或方法


React-Redux
由于全局变量有诸多的缺点，那就换个思路，把store直接集成到React应用的顶层props里面，只要各个子组件能访问到顶层props就行了， 所以react-redux 就用到了react提供的context 
高阶组件。通过高阶组件把store.getState()、store.dispatch、store.subscribe封装起来，子组件对store就无感知了，
子组件正常使用props获取数据以及正常使用callback触发回调，相当于没有store存在一样。


（react-redux提供）
Provider
Provider 其实就只是一个外层容器，使原来整个应用成为Provider的子组件,接收store作为props，通过context对象传递给子孙组件上的connect
Provider把store放到context里，所有的子元素可以直接取到store，本质上 Provider 就是给 connect 提供 store 用的。

（react-redux提供）
connect
connect 是一个高阶组件，接受一个组件 WrappedComponent 作为参数，负责链接组件，通过context获取Provider中的store放到组件的属性里。主要有两个作用：
1. 负责接受一个组件，把state里的一些数据放进去，返回一个组件；2. 数据变化的时候，能够通知组件。

connect函数提供了两个可选参数：（dva简化了这个）
mapStateToProps：每次state变化时都会被调用，接受全局state，需要返回当前组件需要的数据。
mapDispatchToProps：该参数可以是函数或者对象
connect(
  mapStateToProps,
  mapDispatchToProps
)(Component)



副作用（使用中间件 解决异步问题 react-thunk）
中间件的中间是指 Action 和 Store 之间的关系
Action 只能是一个对象派发Store ，这个是在没有使用redux-thunk情况下；使用了redux-thunk ，Action 可以为一个函数 所以Dispatch方法就是Action和Store的中间件，就是对Dispatch方法的封装。
利用react-thunk对D


`redux-thunk`
action 是一个函数 处理异步请求，然后请求完成再dispatch（action）修改store


`redux-saga`
action 还是一个对象， 可以在generator函数内执行异步操作，然后再put(action)改变store中的state



dva 
1、基于 redux、redux-saga 和 react-router 的轻量级前端框架。
好的方面是将state、reducer、effect统一到一起方便管理，
组件通过dispatch去出发action里面的函数，如果是同步的就去进入model的ruducer去修改state，
如果是异步比如fetch获取数据就会被effect拦截（generator写法）通过server交互获取数据进而修改state（yield call异步请求  yield put 提交action修改state），同样state通过connect将model、状态数据与组件相连

弊端：只能使用dva-cli创建项目（不过现在好像可以直接下载dva来使用）
ts兼容较差
effects: {
    // Call、Put、State类型都需自已手动引入
    *fetch(action: {payload: {page: number}}, {call: Call, put: Put}) {
      //使用 yield 后，data 将反射不到 usersService.fetch
      const data = yield call(usersService.fetch, {page: action.payload.page});
      // 这里将触发下面 save reducer，可是它们之间没有建立强关联
      // 如何让这里的 playload 类型与下面 save reducer中的 playload 类型自动约束？
      // 如果下面 save reducer 改名为 save2，如何让这里的 type 自动感应报错？
      yield put({type: "save", payload: data});
    },
  },







why Mobx

我们的项目中使用mobx而不再使用setState原因有如下几个（个人理解）

1 setSate 不是同步操作，当我们通过setstate修改state数据后，后面的逻辑立即取值是旧值，state的新值在下一次render时获取到。
2 setState 不适合管理全局的状态。
