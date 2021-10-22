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