`使用 React Hooks + Context 打造简版 Redux`

`Context`
本意是上下文，它提供一个 Provider 和一个 Consumer，也就是生产者/消费者模式，在某个顶层提供一个 Provider ，下面的子元素通过 Consumer 来消费 Provider 里的数据和方法。

当我们能共享数据后，还剩一个问题就是如何更改 Provider 里的数据呢？答案是：useReducer。

`useReducer 和 redux 的区别`

useReducer 是 useState的代替方案，用于 state 复杂变化
useReducer 是单个组件状态管理，组件通讯还需要 props, 状态由派发的action改变，单向数据流。
redux 是全局状态管理，多组件共享数据




`代码`
<!-- import React, { useContext, createContext } from "react"

const Context = createContext({
  colors: []
})

function Parent() {

  const initState = {
    colors: ["red", "blue"]
  }

  function reducer(state, action) {
    const { colors } = action
    if (action.type === "CHANGE_COLOR") {
      return { colors: colors }
    } else {
      throw new Error()
    }
  }


  const [state, dispatch] = useReducer(reducer, initState)

  return (
    <!-- provide 上的value会覆盖 createContext上的值 -->
    <Context.Provider value={{ colors: state.colors, dispatch: dispatch }}>
      <>
        {/* 假装这些地方有着不同的层级 */}
        <Child1 />
        <Child2 />
      </>
    </Context.Provider>
  )
} -->

<!-- 子组件 -->
<!-- function Child1(props) {
  const { colors, dispatch } = useContext(Context)

  return (
    <div
      style={{ background: colors[0] }}
      onClick={() =>
        dispatch({
          type: "CHANGE_COLOR",
          colors: ["yellow", "blue"]
        })
      }
    >
      I am {colors[0]}
    </div>
  )
} -->


`进阶`
在实际的应用中，我们的业务场景会更复杂，比如我们的数据是动态获取的。
这种情况下你可以把 Provider 抽出来，当 Parent 数据回来之后再初始化 Context


<!-- 定义provide组件 -->

<!-- import React, { useReducer } from 'react'
// 全局 Provider
  export function AppProvider ({reducer, initValue, children}) {
    return (
      <AppContext.Provider value={useReducer(reducer, initValue)}>
        {children}
      </AppContext.Provider>
    )
  }
-->


<!-- 定义context -->

<!-- import React from 'react'
// 创建我们需要的 Context
export const AppContext = React.createContext(null) -->


`组件里使用：`
<!-- import { AppProvider, useAppState } from "./state"

function App() {
  const initState = {
    colors: ["red", "blue"]
  }

  function reducer(state, action) {
    const { colors } = action;
    if (action.type === "CHANGE_COLOR") {
      return { colors: colors };
    } else {
      throw new Error();
    }
  }

  return (
    <AppProvider initValue={initState} reducer={reducer}>
      <div>
        {/* 假装这些地方有着不同的层级 */}
        <Child1 />
        <Child2 />
      </div>
    </AppProvider>
  )
}

function Child1(props) {
  const [state, dispatch] = useAppState()

  return (
    <div
      style={{ background: state.colors[0] }}
      onClick={() =>
        dispatch({
          type: "CHANGE_COLOR",
          colors: ["yellow", "blue"]
        })
      }
    >
      I am {state.colors[0]}
    </div>
  ) -->
}