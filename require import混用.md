import和require如何在项目中混用
2020-04-15 12:32:49
参考地址 干货:import和require如何在项目中混用

虽然我们很清晰的了解了import和require使用上的规范和原则。但是我们去发现我们即使没有遵循规则，我们一样能正常使用。

其实这并不是两套规则能通用，不严谨。

出现这样的问题，要归结与我们的好朋友babel，他在中间做了转换的桥梁，所以我们才能无拘无束的混用他们。



前言
自从js出了es6之后，小伙伴们都在项目中愉快的用起了import,export default,但是，当我们用import方式引入npm第三方模块的时候，却会发现，npm包导出的方式大多是采用了commonjs规范的require,module.exports。这中间babel究竟是如何处理的呢？相信很多小伙伴都有很多问好！那么接下来，我们就来看看这里面的奥秘吧！

module.exports转译
module.exports的使用方法不做过多的说明；来看一下babel编译前和编译后的代码有什么区别。

源文件test.js内容：

1
2
exports.name = "test";
复制代码
经过babel编译后的test.js内容：

1
2
3
"use strict";
exports.name = "test";
复制代码
编译前和编译后，代码并没有多大区别，还是遵循了commonjs规范。

export default转译
关于es6中如何使用export导出模块，这里就不展开叙述了。来看下面一个例子，看看es6的export在经过babel转译之后，会变成什么样子：

源文件test.js内容：

1
2
3
4
5
export var a = "test";
export default function(){
    console.log(1)
}
复制代码
经过babel编译后的test.js内容：

1
2
3
4
5
6
7
8
9
10
11
12
"use strict";
 
Object.defineProperty(exports, "__esModule", {
    value: true
});
 
exports.default = function () {
    console.log(1);
};
 
var a = exports.a = "test";
复制代码
从结果可以看出，babel直接把es6规范的export编译成了commonjs规范的module.exports,并且会给exports对象添加一个__esModule的标记，来表明此对象是从es6的import转过来的（具体有什么作用，接下来会说明）。

import和require
上面两个例子讲了export和module.exports在经过babel编译之后，都变为了commonjs规范的module.exports。接下来我们来说下，在不同场景下，用import和require导入的时候babel是怎么工作的！

场景一：require导入module.exports模块
这个场景是最简单的，因为两者都遵循了commonjs规范。来看下面一个例子： test.js

1
2
3
4
module.exports = {
    name:"西瓜"
}
复制代码
a.js

1
2
3
var a = require("./test");
console.log(a);
复制代码
babel编译以后的a.js

1
2
3
4
5
6
"use strict";
var a = require("./test");
console.log(a);
 
console.log的值:{ name: '西瓜' }
复制代码
该场景不做过多说明。

场景二：require导入export default模块
该场景就是es6和commonjs的混用场景之一了。话不多说，直接上案例。 test.js

1
2
3
4
5
6
export var a = "西瓜";
 
export default function(){
    console.log(1)
}
复制代码
a.js

1
2
3
var a = require("./test");
console.log(a);
复制代码
babel编译以后的a.js

1
2
3
4
5
6
"use strict";
var a = require("./test");
console.log(a);
 
console.log的值:{ default: [Function], a: '西瓜' }
复制代码
为啥是这样的结果呢，上文案例中说了，es6的export模块在被babel编译了以后，就转变成了commonjs的module.exports，所以说，这里require导入export default模块，到后面还是变为require导入module.exports模块了，和场景一一样。

场景三：import导入module.exports模块
这是混用最多的场景了，一般情况下，作为前端人员，写业务代码的时候，更喜欢使用es6自带的export default模块，而发布到npm的第三方包，都是经过编译的，大多遵循了commonjs规范。来看下这种情况下，babel是怎么处理的。

test.js

1
2
3
4
5
module.exports={
    name:"西瓜",
    age:"20"
}
复制代码
a.js

1
2
3
import test from "./test";
console.log(test)
复制代码
babel编译以后的a.js

1
2
3
4
5
6
7
8
9
10
11
12
"use strict";
 
var _test = require("./test");
 
var _test2 = _interopRequireDefault(_test);
 
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
 
console.log(_test2.default);
 
console.log的值:{ name: '西瓜', age: '20' }
复制代码
可以看到，在这种场景下，多了一个_interopRequireDefault方法包装了一层，该方法内容也很简单，判断了传入的对象中是否存在__esModule属性，如果存在则返回原对象；如果不存在则赋值给新对象的default属性；并且console.log中的值变为了default属性；

那这个地方为什么要这么处理呢？
1.问：为什么console.log的值会变为_test2.default？
答：对于es6的import来说，用import xxx from 'xxx'这种方式引入的值就是目标文件所导出的default值，所以这里编译之后console.log的值变成了_test2.default。
2.问：为什么要判断是否存在__esModule？
答：在文章的最前面说过，用export default导出的模块，在经过babel编译后，会给exports加上一个__esModule属性，来证明是es6规范导出的模块。当这里__esModule为true时，说明的该对象由es6规范的export 导出，由于es6的export可以用export default导出默认属性，所以_interopRequireDefault方法直接返回了原对象。当__esModule为false时，说明的该对象由commonjs规范的module.exports导出,在这种情况下，为了遵循es6的import的引入规则，babel在编译时做的一种策略，就是在传入对象的外层做一层default的赋值包装。这样，既保证了import的引入规则，也导出了module.exports的值。

结论
在该场景下，最后console.log的值就是module.exports的值。

场景四：import导入export default模块
这场景就不做过多的解释了，两者都遵循es6规范，直接看文档即可。想要理解这两者在经过babel编译后的引用关系，直接看场景三即可，相信看懂了场景三，结合全文，应该能理解这个过程是怎么一回事了。