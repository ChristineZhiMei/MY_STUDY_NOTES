# 介绍一下 tree shaking 及其工作原理
## 什么是tree shaking
 - 是指通过静态分析源代码，删除未被引用的代码，以减少文件体积，在javaScript中，tree shaking通常与ES6模块捆绑在一起使用，他能有效的帮我移除没有使用过的代码，提高应用的性能和加载速度
 - `ES6`的`import`语法可以完美使用`tree shaking`，因为可以在代码不运行的情况下就能分析出不需要的代码。
 - 因为`tree shaking`只能在静态`modules`下工作。`ECMAScript 6` 模块加载是静态的,因此整个依赖树可以被静态地推导出解析语法树。所以在 `ES6` 中使用 `tree shaking` 是非常容易的。

## Tree shaking的原理是什么？
 - `ES6 Module`引入进行静态分析，故而编译的时候正确判断到底加载了那些模块
 -  静态分析程序流，判断那些模块和变量未被使用或者引用，进而删除对应代码

# common.js 和 es6 中模块引入的区别？
`CommonJS` 是一种模块规范，最初被应用于 `Nodejs`，成为 `Nodejs` 的模块规范。运行在浏览器端的 `JavaScript` 由于也缺少类似的规范，在 `ES6` 出来之前，前端也实现了一套相同的模块规范 (例如: `AMD`)，用来对前端模块进行管理。自 `ES6` 起，引入了一套新的 `ES6 Module` 规范，在语言标准的层面上实现了模块功能，而且实现得相当简单，有望成为浏览器和服务器通用的模块解决方案。但目前浏览器对 `ES6 Module` 兼容还不太好，我们平时在 `Webpack` 中使用的 `export` 和 `import`，会经过 `Babel` 转换为 `CommonJS` 规范。在使用上的差别主要有：
1. `CommonJS` 模块输出的是一个值的拷贝，`ES6` 模块输出的是值的引用。
2. `CommonJS` 模块是运行时加载，`ES6` 模块是编译时输出接口。
3. `CommonJs` 是单个值导出，`ES6 Module`可以导出多个
4. `CommonJs` 是动态语法可以写在判断里，`ES6 Module` 静态语法只能写在顶层
5. `CommonJs` 的 `this` 是当前模块，`ES6 Module`的 `this` 是 `undefined`

# 说一说你对vite的理解，什么是bundleless
1. 早期的模块化commonjs、amd、cmd 早期的浏览器支持的esm（es module）
2. 正式有了esm的浏览器的支持，才有了bundleless方案
3. bundleless打包，webpack是需要通过模块化规范支持，依赖分析完后构建依赖图depGraph。bundless提倡少打包，不打包
4. 打包js（ts、tsx、jsx、vue） css png svg 等输出到某一个指定目录
5. `<script type='module' src=...><script>`
6. 执行过程是栈的过程，先解析引用，入栈，再出栈使用
7. 热更新需要开发 开发服务器
8. 产物的构建机制
		产物的构建不是依赖打包工具，而是依赖 **编译** 工具
		webpack里面要编译 js 需要 babel
		 - 开发环境下，js 不用打包是直出的，打包的是代码资源，tsx等，css,字体资源
			 - 使用esbuild打包代码资源
			 - 使用postcss处理css
			 - 图片字体等使用类似于webpack的file node处理
			 - 需要针对需要打包的资源做转换的处理
			插件化机制来实现**vite-plugin-xxx**
			 - 优化
				 - 对已经编译的进行缓存
				 - 增量编译
				 - HMR
			 - 缺点：dts文件需要自行处理，es5以下不支持
		 - 构建环境
			 - 使用rollup打包
			 - 优化
				 - 构建工具来处理tree-shaking（是构建工具用的不是vite用的）

# vite 的构建过程了解吗，说说其实现原理
开发构建
 - vite命令执行
 - 项目初始化：读取分析vite.config.js配置文件
 - 启动开发服务：基于express启动HTTP服务器
 - ESM支持：利用浏览器原生 ESM 进行模块加载
 - 按需编译：实时编译请求的模块
 - HMR（热更新）：通过WebSocket实现模块的局部更新
 - Source Maps：自动生成 Source Maps, 便于调试
打包
 - vite build
 - 项目初始化：读取配置文件vite.config.js
 - 入口解析：使用Rollup 构建模块依赖图
 - 插件处理：通过插件系统进行代码转换、压缩和资源处理
 - Tree Shaking：移除未使用的代码
 - 代码拆分：将代码拆分成多个模块
 - 输出内容：打包生成最终的输出文件
 - 资源优化：优化CSS和静态资源
 - 缓存处理：为静态资源添加内容哈希，便于缓存管理