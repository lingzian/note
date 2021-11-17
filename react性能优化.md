React 性能优化思路
我觉得React 性能优化的理念的主要方向就是这两个：

1 减少重新 render 的次数。因为在 React 里最重(花时间最长)的一块就是 reconciliation(简单的可以理解为 diff)，如果不 render，就不会 reconciliation。

2 减少计算的量。主要是减少重复计算，对于函数式组件来说，每次 render 都会重新从头开始执行函数调用。

一、使用React.memo
  1 控制子组件的更新，取决于props是否相同
  2 默认情况下其只会对 props 的复杂对象做浅层对比(浅层对比就是只会对比前后两次 props 对象引用是否相同，不会对比对象里面的内容是否相同)，如果你想要控制对比过程，那么请将自定义的比较函数通过第二个参数传入来实现。
  function MyComponent(props) {
    /* 使用 props 渲染 */
  }
  function areEqual(prevProps, nextProps) {
    /*
    如果把 nextProps 传入 render 方法的返回结果与
    将 prevProps 传入 render 方法的返回结果一致则返回 true，
    否则返回 false
    */
  }
  export default React.memo(MyComponent, areEqual);

类组件使用shouldComponentUpdate对比state 和 props是否要重新渲染

二、使用useMemo缓存大量的计算， useCallback存储要传给子组件的方法



三、 少使用内联对象或者函数
因为父组件更新，会重新创建新的对象，如果使用了，可以搭配react.memo使用

四、调整CSS而不是强制组件加载和卸载
一些动画效果，尽量用css去控制，而不是一直销毁然后重新加载


五、大多数打包器（比如 webpack、rollup、browserify）的作用就是把你的页面代码打包成一个很大的 “bundle”，所有的代码都会在这个 bundle 中。但是，随着应用的复杂度日益提高，bundle 的体积也会越来越大，加载 bundle 的时间也会变长，这就对加载过程中的用户体验造成了很大的负面影响。

最初写法
import { add } from './math';
console.log(add(16, 26));

改成
import("./math").then(math => {
  console.log(math.add(16, 26));
});

相当于懒加载代码,