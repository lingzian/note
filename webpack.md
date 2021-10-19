webpack 



`webpack(静态模块打包器)`

1 入口文件 ==> index.js ==>(引入各种东西 React.js xx.less) 等等 就形成一个chunk(代码块)

chunk  ==>  less 转变成 css    
       ==>  es6 装换成 js
       统称打包
打包完之后输出资源 就叫做bundel

对于一份同逻辑的代码，当我们手写下一个一个的文件，它们无论是 ESM 还是 commonJS 或是 AMD，他们都是 module ；
当我们写的 module 源文件传到 webpack 进行打包时，webpack 会根据文件引用关系生成 chunk 文件，webpack 会对这个 chunk 文件进行一些操作；
webpack 处理好 chunk 文件后，最后会输出 bundle 文件，这个 bundle 文件包含了经过加载和编译的最终源文件，所以它可以直接在浏览器中运行。
一般来说一个 chunk 对应一个 bundle

module，chunk 和 bundle 其实就是同一份逻辑代码在不同转换场景下的取了三个名字：

我们直接写出来的是 module，webpack 处理时是 chunk，最后生成浏览器可以直接运行的 bundle。







`webpack五个核心`

Entry 
    入口文件 （webpack从哪个文件开始打包）

Output
    输出指示webpack打包后的资源bundles输出到哪里去，怎么命名
Loader
    处理非js的文件  css img 翻译成js

Plugins
    插件 打包优化压缩 定义环境变量

mode
    development: 会将process.env.NODE_ENV 的值设置为 development  启用 NamedChunksPlugin 和NamedModulesPlugin
    production : 会将 process.env.NODE_ENV 的值设置为 production  启用 FlagDepencyUsagePlugin


结论： 
    1：webpack能处理 js/json资源，不能处理css/img等其他资源
    2： 生产环境和开发环境将es6模块化编译成浏览器能识别的模块化
    3：生产环境比开发环境多一个压缩js代码


使用 output 中 path会使用到  const {resolve} = require('path')  用来拼接绝对路径， 
path: resolve(__dirname, 'build')  __dirname是node的变量 ，代表当前文件的绝对路径

loader: 1下载 2使用
plugins: 1下载 2引入 3使用

loader的配置

module: {
    rules: [
        详细的loader配置
        {
            test: /\.css/
            use: ['style-loader','css-loader']
            style-loader', 创建style标签，将js中的样式资源插入进行，添加到head中生效
            // 用MiniCssExtractPlugin.loader 取代style-loader 提取js中的css成单独文件 （避免加载闪屏，css以link标签引入，减少build.js体积，加载更快）
            MiniCssExtractPlugin.loader,
        '   css-loader' 将css变成common.js模块加载js中， 里面内容是样式字符串
        },
        {
            loader: 'postcss-loader',
            options: {
                ident: 'postcss',
                plugins: () => [
                    //postcss的插件 帮postcss找到package.json中browserslist里面的配置，加载css兼容性样式
                    require('postcss-preset-env')()
                ]
            }
        }
        
        // 使用一个loader
        // 处理图片资源  处理不了html中的img图片
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
            // 图片大小小于8kb就会被base64处理
            优点： 减少请求  缺点 图片体积变大
            limit: 8*1024
            // 问题url-loader使用es6模块解析，而html-loader引入图片是common.js
            // 解析时会出现[object Module] 
            // 解决：关闭url-loader的es6模块化
            esModule: false,
            name: '[hash:10].[ext]'  [hash:10]取图片的hash的前十位   [ext]取文件原来的扩展名
        }

        {
            test: /\,html$/,
            // 处理html文件的img图片（负责引入img，从而能被url-loader处理）
            loader: 'html-loader'
        }
    ]
}


`plugins 配置`
const HtmlWebpackPlugin = require('html-webpack-plugin');
plugins: [
    默认会创建一个空的HTML，引入打包输出的所有资源（js/css）
 new HtmlWebpackPlugin({
     template: './src/index.html'  // 复制'./src/index.html' 文件并引入打包输出的所有资源（js/css）
 })  
 // 用MiniCssExtractPlugin.loader 取代style-loader 提取js中的css成单独文件 （避免加载闪屏，css以link标签引入，减少build.js体积，加载更快）
 new MiniCssExtractPlugin{
     filename: 'css/build.css
 }
 // 压缩css
 new OptimizeCssAssetsWebpackPlugins()
 // eslint... 检查代码
 // babel... 兼容js
]

// 开发服务器，用来自动化（自动编译，自动打开浏览器，自动刷新浏览器）
devServer: {
    // 项目构建后路径
    contentBase： resolve（——dirname,'build'）,
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    // 自动打开浏览器
    open: true
}



 `webpack 优化`

 ## 开发环境性能优化
* 优化打包构建速度
  * HMR
* 优化代码调试
  * source-map (定位问题代码)

## 生产环境性能优化
* 优化打包构建速度
  * oneOf
  * babel缓存
  * 多进程打包
  * externals
  * dll
* 优化代码运行的性能
  * 缓存(hash-chunkhash-contenthash)
  * tree shaking
  * code split
  * 懒加载/预加载
  * pwa



  HMR
  /*
  HMR: hot module replacement 热模块替换 / 模块热替换
    作用：一个模块发生变化，只会重新打包这一个模块（而不是打包所有模块） 
      极大提升构建速度
      
      样式文件：可以使用HMR功能：因为style-loader内部实现了~
      js文件：默认不能使用HMR功能 --> 需要修改js代码，添加支持HMR功能的代码 每个modulet 加上hot属性 （有的话证明开启了hrm功能）
        注意：HMR功能对js的处理，只能处理非入口js文件的其他文件。因为入口文件引入了很多东西，入口文件改变就会重新引入所有东西
      html文件: 默认不能使用HMR功能.同时会导致问题：html文件不能热更新了~ （不用做HMR功能）
        解决：修改entry入口，将html文件引入
*/



`source-map`

/*
  source-map: 一种 提供源代码到构建后代码映射 技术 （如果构建后代码出错了，通过映射可以追踪源代码错误）

    [inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map

    source-map：外部
      错误代码准确信息 和 源代码的错误位置
    inline-source-map：内联
      只生成一个内联source-map
      错误代码准确信息 和 源代码的错误位置
    hidden-source-map：外部
      错误代码错误原因，但是没有错误位置
      不能追踪源代码错误，只能提示到构建后代码的错误位置
    eval-source-map：内联
      每一个文件都生成对应的source-map，都在eval
      错误代码准确信息 和 源代码的错误位置
    nosources-source-map：外部
      错误代码准确信息, 但是没有任何源代码信息
    cheap-source-map：外部
      错误代码准确信息 和 源代码的错误位置 
      只能精确的行
    cheap-module-source-map：外部
      错误代码准确信息 和 源代码的错误位置 
      module会将loader的source map加入

    内联 和 外部的区别：1. 外部生成了文件，内联没有 2. 内联构建速度更快

    开发环境：速度快，调试更友好
      速度快(eval>inline>cheap>...)
        eval-cheap-souce-map
        eval-source-map
      调试更友好  
        souce-map
        cheap-module-souce-map
        cheap-souce-map

      --> eval-source-map  / eval-cheap-module-souce-map

    生产环境：源代码要不要隐藏? 调试要不要更友好
      内联会让代码体积变大，所以在生产环境不用内联
      nosources-source-map 全部隐藏
      hidden-source-map 只隐藏源代码，会提示构建后代码错误信息

      --> source-map / cheap-module-souce-map
*/







/*
  `缓存：`
    babel缓存
      cacheDirectory: true
      --> 让第二次打包构建速度更快
    文件资源缓存
      hash: 每次wepack构建时会生成一个唯一的hash值。
        问题: 因为js和css同时使用一个hash值。
          如果重新打包，会导致所有缓存失效。（可能我却只改动一个文件）
      chunkhash：根据chunk生成的hash值。如果打包来源于同一个chunk，那么hash值就一样
        问题: js和css的hash值还是一样的
          因为css是在js中被引入的，所以同属于一个chunk
      contenthash: 根据文件的内容生成hash值。不同文件hash值一定不一样    
      --> 让代码上线运行缓存更好使用
*/





/*
  tree shaking：去除无用代码
    前提：1. 必须使用ES6模块化  2. 开启production环境
    作用: 减少代码体积

    在package.json中配置 
      "sideEffects": false 所有代码都没有副作用（都可以进行tree shaking）
        问题：可能会把css / @babel/polyfill （副作用）文件干掉
      "sideEffects": ["*.css", "*.less"]
*/






//代码分割   能够分离公共代码，只需要加载一次
  /*
    1. 可以将node_modules中代码 或者代码中又动态加载或者异步加载的代码 单独打包一个chunk最终输出
    2. 自动分析多入口chunk中，有没有公共的文件。如果有会打包成单独一个chunk
  */
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  },




hash (文件指纹)
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


`文件监听原理呢？`
在发现源码发生变化时，自动重新构建出新的输出文件。
Webpack开启监听模式，有两种方式：

启动 webpack 命令时，带上 --watch 参数在配置 webpack.config.js 中设置 watch:true
缺点：每次需要手动刷新浏览器
原理：轮询判断文件的最后编辑时间是否变化，如果某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等 aggregateTimeout 后再执行。


`说一下 Webpack 的热更新原理吧`
Webpack 的热更新又称热替换（Hot Module Replacement），缩写为 HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。
HMR的核心就是客户端从服务端拉去更新后的文件，准确的说是 chunk diff (chunk 需要更新的部分)，实际上 WDS 与浏览器之间维护了一个 Websocket，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起 Ajax 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起 jsonp 请求获取该chunk的增量更新。
后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 HotModulePlugin 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像react-hot-loader 和 vue-loader 都是借助这些 API 实现 HMR。



`模块打包原理知道吗？`
Webpack 实际上为每个模块创造了一个可以导出和导入的环境，本质上并没有修改 代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致。



`Webpack构建流程简单说一下`
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数
开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译
确定入口：根据配置中的 entry 找出所有的入口文件
编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。
简单说

初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler
编译：从 Entry 出发，针对每个 Module 串行调用对应的 Loader 去翻译文件的内容，再找到该 Module 依赖的 Module，递归地进行编译处理
输出：将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中




`使用webpack开发时，你用过哪些可以提高效率的插件？`
HotModuleReplacementPlugin   模块热替换 (css 改变 无感知更新页面)



`webpack 中 loader 和 plugin 的区别是什么`
loader，它是一个转换器，将A文件进行编译成B文件，比如：将A.less转换为A.css，单纯的文件转换过程。
plugin是一个扩展器，它丰富了webpack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务(打包优化、文件管理、环境注入等)