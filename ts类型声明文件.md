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