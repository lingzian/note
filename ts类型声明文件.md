ts
1、 下载的包就自带类型声明 (与npm一同发布)
2、 下载的包没有自带类型声明，可以去社区找（jquery npm install @types/jquery --save-dev）
3、 自己声明 在全局typing.d.ts
declare : 用来定义一些全局的 变量、方法、类等等 
interface 和 type 用来声明全局类型

例如我们项目中套壳 apicloud对象的使用 就要自己声明类型



tsconfig.ts 配置
typeRoots： 默认值为node_modules/@types （所以下载了@types/jquery 就会自动去node_modules/@types找类型）
types： 配合typeRoots来使用，用于指定需要包含的模块，只有在这里列出的模块的声明文件才会被加载进来,



<!-- 面试 -->
`js项目如何升级为ts？有何影响？`
1 从0开始建一个TS项目（脚手架选择ts类型）
2（手动改项目的话） 全局安装ts， 初始化tsconfig.json文件， 寻找项目中所使用的的库的类型包然后下载，把文件后缀改成ts，下载@babel/preset-typescript等等有关解析ts的babel插件，然后在webpack里面一些loader的检查添加ts文件检查


`ts 基础类型都哪些，他们跟js的区别`
js就有的 ----boolean,number,string,Null 和 Undefined，数组，Object
js没有的 ----Tuple(数组已知类型和数量),enum(类似一个对象，不赋值情况下永远是0),Any(任何类型),Void(没有任何类型，常用于函数无返回值),Never(永不存在得值类型),类型断言（xxx as 类型）


`ts为什么会流行？与ECMA新规范的关系？`
在静态类型检查的帮助下，即使是上百行的代码，也能在编译阶段就发现问题，工程质量和工作效率疯狂提升。
它提供了可选静态类型的优点。在这里，Typescript提供了可以添加到变量、函数、属性等的类型。
Typescript能够编译出一个能在所有浏览器上运行的JavaScript版本。
TypeScript总是在编译时强调错误，而JavaScript在运行时指出错误。
TypeScript支持强类型或静态类型，而这不是在JavaScript中。
它有助于代码结构。
它使用基于类的面向对象编程。
它提供了优秀的工具支持和智能感知，后者在添加代码时提供活动提示。
它通过定义模块来定义名称空间概念。


`如何理解接口，泛型?`
泛型是指在定义函数、接口或类的时候，不预先指定具体的类型，使用时再去指定类型的一种特性。可以把泛型理解为代表类型的参数
接口是指定义一些参数，规定变量里面有什么参数，参数是什么类型，使用时就必须有这些对应类型的参数，少或者多参数、参数类型不对都会报错


`Typescript的缺点是什么?`
如果我们在浏览器中运行TypeScript应用程序，需要一个编译步骤将TypeScript转换成JavaScript
要使用任何第三方库，必须使用定义文件。并不是所有第三方库都有可用的定义文件
类型定义文件的质量是一个问题，即如何确保定义是正确的?


`Typescript中的名称空间是什么？`
名称空间是用于对功能进行逻辑分组的一种方式。名称空间用于在内部维护typescript的遗留代码。它封装了共享某些关系的特性和对象。名称空间也称为内部模块。名称空间还可以包括接口、类、函数和变量，以支持一组相关功能。


`JavaScript不支持函数重载，但TypeScript是否支持函数重载？`
是的，TypeScript支持函数重载。但是它的实现很奇怪，当我们在TypeScript中执行函数重载时，我们只能实现一个带有多个签名的函数。
function add(a:string,b:string):string
function add(a:number,b:number):number

`什么是TypeScript Declare关键字?`
我们知道所有的JavaScript库/框架都没有TypeScript声明文件，但是我们希望在TypeScript文件中使用它们时不会出现编译错误。为此，我们使用declare关键字。在我们希望定义可能存在于其他地方的变量的环境声明和方法中，可以使用declare关键字。

例如，假设我们有一个名为myLibrary的库，它没有TypeScript声明文件，在全局命名空间中有一个名为myLibrary的命名空间。如果我们想在TypeScript代码中使用这个库，我们可以使用以下代码:
declare var myLibrary
TypeScript运行时将把myLibrary变量赋值为任意类型。这是一个问题，我们不会得到智能感知在设计时，但我们将能够使用库在我们的代码。


`什么是tsconfig.son文件?`
tsconfig.son文件是json格式的文件。tsconfig.json文件中，我们可以指定各种选项告诉编译器如何编译当前项目。目录中存在tsconfig.json文件，表明该目录是TypeScript项目的根目录。 下面是一个示例tsconfig.json文件。


`TS的“interface”和“type”语句有什么区别？`
接口	                                                      Type类型
1	描述对象或者函数	                                         不同于 interface 只能定义对象类型，type 声明还可以定义基础类型、联合类型（|）或交叉类型（&）。
2	接口可以在extends或implements子句中命名。	                  对象类型文字的类型别名不能在“扩展”或“实现”子句中命名。
3	接口创建一个到处使用的新名称。	                             类型别名不创建新名称。
4	一个接口可以有多个合并声明。	                              对象类型字面量的类型别名不能有多个合并声明。



`什么是TypeScript中的类型断言？`
类型断言的工作方式类似于其他语言中的类型转换，但是它不像其他语言一样执行C＃和Java那样的类型检查或数据重组。类型转换附带运行时支持，而类型断言对运行时没有影响。但是，类型断言仅由编译器使用，并向编译器提供有关我们希望如何分析代码的提示。

`TypeScript的as语法是什么？`
as是TypeScript中类型断言的附加语法，引入as-语法的原因是原始语法(<type>)与JSX冲突。


`什么是JSX？我们可以在TypeScript中使用JSX吗？`
即javscript XML，是js内定义的一套XML语法
JSX只不过是带有不同扩展名的Javascript。
JSX是一种可嵌入的类似xml的语法。它将被转换成有效的JavaScript。JSX随着React框架而流行起来。TypeScript支持嵌入、类型检查和直接将JSX编译成JavaScript。


`什么是Rest参数？`
rest参数用于向函数传递零个或多个值。它是通过在参数前加上三个点字符(‘…’)来声明的。它允许函数在不使用arguments对象的情况下拥有可变数量的参数。当我们有不确定数量的参数时，这是非常有用的。

rest参数要遵循的规则:

一个函数中只允许有一个rest参数。
它必须是数组类型。
它必须是参数列表中的最后一个参数。


`解释TypeScript的Enum枚举类型？`
枚举或枚举是一种数据类型，允许我们定义一组命名常量。使用枚举可以更容易地记录意图，或者创建一组不同的案例。它是相关值的集合，可以是数值或字符串值。

enum G {
  a,
  b,
  c,
  d
}
G.a = 0
G[0] = a


`解释相对模块和非相对模块的导入`
非相对导入可以相对于baseUrl解析，也可以通过路径映射解析。换句话说，我们在导入任何外部依赖项时使用非相对路径。 例子: import * as $ from “jquery”; import { Component } from “@angular/core”;

相对导入可以用于我们自己的模块，这些模块保证在运行时维护它们的相对位置。相对导入以/、./或../开头。 例子: import Entry from “./components/Entry”; import {DefaultHeaders} from “../constants/http”;


`implements 与 extends 的区别`
extends, 子类会继承父类的所有属性和方法。不可以重写属性，但可以重写方法
implements，使用implements关键字的类将需要实现需要实现的类的所有属性和方法。


`unknown, any的区别`
任何类型的值都可以赋值给any 或者unknow
any类型的值可以赋值给任何类型的变量;unknown类型的值只能给unknown和any类型的变量



`说说你对 typescript 的理解？与 javascript 的区别？`
TypeScript 是 JavaScript 的类型的超集
其是一种静态类型检查的语言，提供了类型注解，在代码编译阶段就可以检查出数据类型的错误



`说说你对 TypeScript 中高级类型的理解？有哪些？`
除了string、number、boolean 这种基础类型外，在 typescript 类型声明中还存在一些高级的类型应用
这些高级类型，是typescript为了保证语言的灵活性，所使用的一些语言特性。这些特性有助于我们应对复杂多变的开发场景
- 交叉类型 （& 将多个类型合并为一个类型）
- 联合类型 （| 联合类型的语法规则和逻辑 “或” 的符号一致，表示其类型为连接的多个类型中的任意一个）
- 类型别名
type some = boolean | string
- 类型索引 （keyof 类似于 Object.keys ，用于获取一个接口中 Key 的联合类型。）
interface Button {
    type: string
    text: string
}
type ButtonKeys = keyof Button
// 等效于
type ButtonKeys = "type" | "text"
- 类型约束 （通过关键字 extend 进行约束，不同于在 class 后使用 extends 的继承作用，泛型内使用的主要作用是对泛型加以约束）
type BaseType = string | number | boolean

// 这里表示 copy 的参数
// 只能是字符串、数字、布尔这几种基础类型
function copy<T extends BaseType>(arg: T): T {
  return arg
}
- 映射类型 （通过 in 关键字做类型的映射，遍历已有接口的 key 或者是遍历联合类型，如下例子：）
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

interface Obj {
  a: string
  b: string
}
type ReadOnlyObj = Readonly<Obj>
上述的结构，可以分成这些步骤：

keyof T：通过类型索引 keyof 的得到联合类型 'a' | 'b'
P in keyof T 等同于 p in 'a' | 'b'，相当于执行了一次 forEach 的逻辑，遍历 'a' | 'b'
所以最终ReadOnlyObj的接口为下述：
interface ReadOnlyObj {
    readonly a: string;
    readonly b: string;
}
- 条件类型（条件类型的语法规则和三元表达式一致，经常用于一些类型不确定的情况。）
T extends U ? X : Y
如果 T 是 U 的子集，就是类型 X，否则为类型 Y



`说说你对 TypeScript 装饰器的理解？应用场景？`
装饰器是一种特殊类型的声明，它能够被附加到类声明，方法， 访问符，属性或参数上是一种在不改变原类和使用继承的情况下，动态地扩展对象功能同样的，本质也不是什么高大上的结构，就是一个普通的函数.对原来功能进行扩展



`说说对 TypeScript 中命名空间与模块的理解？区别？`
- 模块
TypeScript 与 ECMAScript 2015 一样，任何包含顶级 import 或者 export 的文件都被当成一个模块
- 命名空间
命名空间一个最明确的目的就是解决重名问题
命名空间本质上是一个对象，作用是将一系列相关的全局变量组织到一个对象的属性
- 区别
命名空间是位于全局命名空间下的一个普通的带有名字的 JavaScript 对象，使用起来十分容易。但就像其它的全局命名空间污染一样，它很难去识别组件之间的依赖关系，尤其是在大型的应用中
像命名空间一样，模块可以包含代码和声明。 不同的是模块可以声明它的依赖
在正常的TS项目开发过程中并不建议用命名空间，但通常在通过 d.ts 文件标记 js 库类型的时候使用命名空间，主要作用是给编译器编写代码的时候参考使用