`JavaScript中Function.prototype.toString.apply()报错？`
我在高程3看到，此书中写明了String、Function、Array中原型的toString方法都是重写过的，所以toString真正意义上最原始的，是Object中的toString 方法。
Funciton.prototype中的重写toString方法中应该规定了传入的this值的类型必须是函数类型，也就是说重写的toString方法中做了这样一个判断 => Object.prototype.toString.call(this) === '[object Function]'  结果为true时，才会允许你继续执行下去，否则返回一个错误，错误如下：
# Uncaught TypeError: Function.prototype.toString requires that 'this' be a Function


`有以下 3 个判断数组的方法，请分别介绍它们之间的区别和优劣Object.prototype.toString.call() 、 instanceof 以及 Array.isArray()`
Object.prototype.toString.call() 常用于判断浏览器内置对象时。

[]  instanceof Array; // true
instanceof  的内部机制是通过判断对象的原型链中是不是能找到类型的 prototype。

Array.isArray()用来判断对象是否为数组

nstanceof 与 isArray当检测Array实例时，Array.isArray 优于 instanceof ，因为 Array.isArray 可以检测出 iframes
Array.isArray() 与 Object.prototype.toString.call()Array.isArray()是ES5新增的方法，当不存在 Array.isArray() ，可以用 Object.prototype.toString.call() 实现


`闭包的实现原理和作用`
闭包的概念：指有权访问另一个函数作用域中的变量的函数，一般情况就是在一个函数中包含另一个函数。
闭包的作用：访问函数内部变量、保持函数在环境中一直存在，不会被垃圾回收机制处理
3、闭包的优点：
方便调用上下文中声明的局部变量
逻辑紧密，可以在一个函数中再创建个函数，避免了传参的问题
4、闭包的缺点：
因为使用闭包，可以使函数在执行完后不被销毁，保留在内存中，如果大量使用闭包就会造成内存泄露，内存消耗很大


`作用域的理解?`
作用域决定了代码区块中变量的可访问性（侧面描写出变量的生命周期）
全局作用域
函数作用域
词法作用域
块级作用域
..


`谈谈iframe的优缺点`
iframe的优点：
1.iframe能够原封不动的把嵌入的网页展现出来。

2.如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。

3.网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用。

4.如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决。

iframe的缺点：
1.会产生很多页面，不容易管理。

2.iframe框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差。

3.代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理iframe中的内容，所以使用iframe会不利于搜索引擎优化。

4.很多的移动设备（PDA 手机）无法完全显示框架，设备兼容性差。

5.iframe框架页面会增加服务器的http请求，对于大型网站是不可取的。

分析了这么多，现在基本上都是用Ajax来代替iframe，所以iframe已经渐渐的退出了前端开发。




`0.1+0.2 ！== 0.3`
计算机存储双精度浮点数需要先把十进制数转换为二进制的科学记数法的形式，然后计算机以自己的规则{符号位+(指数位+指数偏移量的二进制)+小数部分}存储二进制的科学记数法

因为存储时有位数限制（64位），并且某些十进制的浮点数在转换为二进制数时会出现无限循环，会造成二进制的舍入操作(0舍1入)，当再转换为十进制时就造成了计算误差



`BFC 定义` 块级格式化上下文
它是一个独立的渲染区域 它规定了内部的Box如何布局，并且与这个区域外部毫不相干。
触发BFC的条件
·body根元素
·设置浮动，不包括none
·设置定位，absoulte或者fixed
·行内块显示模式，inline-block
·设置overflow，即hidden，auto，scroll
·表格单元格，table-cell
·弹性布局，flex

解决问题 
1、解决外边距的重叠，两个元素设置了margin会重叠，但是把他们设置为bfc即可解决
2、解决包含塌陷 子元素添加margintop 会把父元素一起带跑。把父元素设置为bfc即可解决
3、父元素没有设置高度，子元素全部设置为浮动，父元素的高度不见了。把父元素设置为bfc即可解决
4、BFC可以阻止标准流元素被浮动元素覆盖



`CSS盒子模型`
标准盒子模型
总宽度 = 宽度+左填充+右填充+左边框+右边框+左边距+右边距
width = width
怪异盒子模型
总宽度 = 宽度（内容+边框+左右填充）+左右边距
width = width+padding

box-sizing属性的三个值 ：
content-box： 默认值，border和padding不算到width范围内，采用标准模式解析计算，是W3c的标准模型(default)
border-box：border和padding划归到width范围内，采用怪异模式解析计算，是IE的怪异盒模型
padding-box：将padding算入width范围





`promise问题`
1、Promise 对象后面要跟catch()方法，这样可以处理 Promise 内部发生的错误。catch()方法返回的还是一个 Promise 对象，因此后面还可以接着调用then()方法。一般情况下then(()=>{},()=>{})第二个参数 reject的函数不添加，而是在then后面catch()能捕获正哥流程的错误。catch能接收中途的reject或者throw new Error

2、promise只要返回了结果，后面的then就会一直执行下去，所以 then 如何中断？
then 块默认会向下顺序执行，return 是不能中断的，可以通过 throw 来跳转至 catch 实现中断。

3、什么时候适合用 Promise 而不是传统回调函数？
当需要多次顺序执行异步操作的时候，

4、什么时候我们需要再写一个 then 而不是在当前的 then 接着编程？
当你又需要调用一个异步任务的时候。

缺点：无法取消 Promise ，错误需要通过回调函数来捕获




` Async/await`
优点是：代码清晰，不用像 Promise 写一大堆 then 链，处理了回调地狱的问题

缺点：await 将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用 await 会导致性能上的降低。