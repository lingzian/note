webpack 

hash
`hash`
  hash代表每次webpack编译中生成的hash值，所有使用这种方式的文件hash都相同。

`chunkhash`
  chunkhash基于入口文件及其关联的chunk形成，某个文件的改动只会影响与它有关联的chunk的hash值，不会影响其他文件.
  一个 index.js文件 里面 引入了 index.css和common.js 那么打包生成的 index.bundle.css 和 index.bundle.js 都属于 chunk 0。


 `contenthash`
 contenthash表示由文件内容产生的hash值，内容不同产生的contenthash值也不一样。在项目中，通常做法是把项目中css都抽离出对应的css文件来加以引用。
 



 `webpack 离线缓存静态资源如何实现`

在配置webpack时，我们可以使用html-webpack-plugin来注入到和html一段脚本来实现将第三方或者共用资源进行 静态化存储在html中注入一段标识，例如 <% HtmlWebpackPlugin.options.loading.html %> ,在 html-webpack-plugin 中即可通过配置html属性，将script注入进去
利用 webpack-manifest-plugin 并通过配置 webpack-manifest-plugin ,生成 manifestjson 文件，用来对比js资源的差异，做到是否替换，当然，也要写缓存script
在我们做Cl以及CD的时候，也可以通过编辑文件流来实现静态化脚本的注入，来降低服务器的压力，提高性能
可以通过自定义plugin或者html-webpack-plugin等周期函数，动态注入前端静态化存储script


`webpack如何实现持久化缓存`

服务端设置http缓存头（cache-control）
打包依赖和运行时到不同的chunk，即作为splitChunk,因为他们几乎是不变的
延迟加载：使用import()方式，可以动态加载的文件分到独立的chunk,以得到自己的chunkhash
保持hash值的稳定：编译过程和文件内通的更改尽量不影响其他文件hash的计算，对于低版本webpack生成的增量数字id不稳定问题，可用hashedModuleIdsPlugin基于文件路径生成解决

提取公共第三⽅库: SplitChunksPlugin插件来进⾏公共模块抽取,利⽤浏览器缓存可以⻓期缓存这些⽆需频繁变动的公共代码