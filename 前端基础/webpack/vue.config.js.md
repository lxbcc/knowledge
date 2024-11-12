[安全验证 - 知乎](https://zhuanlan.zhihu.com/p/426181790)
```js
module.exports = {
    // 部署生产环境和开发环境下的URL,baseUrl 从 Vue CLI 3.3 起已弃用，请使用publicPath
    publicPath: process.env.NODE_ENV === "production" ? "/" : "/",
    // outputDir: 在npm run build 或 yarn build 时 ，生成文件的目录名称
    outputDir: "mycli3",
    //用于放置生成的静态资源 (js、css、img、fonts) 的；（项目打包之后，静态资源会放在这个文件夹下）
    assetsDir: "assets",
    //默认情况下，生成的静态资源在它们的文件名中包含了 hash 以便更好的控制缓存。你可以通过将这个选项设为 false 来关闭文件名哈希。(false的时候就是让原来的文件名不改变)
    filenameHashing: false,
    // lintOnSave：{ type:Boolean default:true } 是否使用eslint
    lintOnSave: true,
    //如果你想要在生产构建时禁用 eslint-loader，你可以用如下配置
    // lintOnSave: process.env.NODE_ENV !== 'production',
    
    //是否使用包含运行时编译器的 Vue 构建版本。设置为 true 后你就可以在 Vue 组件中使用 template 选项了，但是这会让你的应用额外增加 10kb 左右。(默认false)
    // runtimeCompiler: false,
    
    /**
    * 如果你不需要生产环境的 source map，可以将其设置为 false 以加速生产环境构建。
    * 打包之后发现map文件过大，项目文件体积很大，设置为false就可以不输出map文件
    * map文件的作用在于：项目打包后，代码都是经过压缩加密的，如果运行时报错，输出的错误信息无法准确得知是哪里的代码报错。
    * 有了map就可以像未加密的代码一样，准确的输出是哪一行哪一列有错。
    * */
    productionSourceMap: false,

    // css相关配置
    css: {
        extract: true, // 是否使用css分离插件 ExtractTextPlugin
        sourceMap: false, // 是否为 CSS 开启 source map。设置为 true 之后可能会影响构建的性能
        loaderOptions: {
            less: {
                javascriptEnabled: true //less 配置
            }
        }, // css预设器配置项
        modules: false // 启用 CSS modules for all css / pre-processor files.
    }
    
    // 支持webPack-dev-server的所有选项(https://webpack.js.org/configuration/dev-server/)
    devServer: {
        host: "0.0.0.0",
        port: 8080, // 端口号
        https: false, // https:{type:Boolean}
        open: true, //配置自动启动浏览器
        // 设置请求头
        headers: {
            "Access-Control-Allow-Origin": "*"
        },
        // proxy: 'http://localhost:4000' // 配置跨域处理,只有一个代理
        // 配置多个代理
        proxy: {
            "/api": {
            target: "http://192.168.x.xxx:8090", // 要访问的接口域名
            ws: true, // 是否启用websockets
            changeOrigin: true, //开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
            pathRewrite: {
                "^/api": "" //这里理解成用'/api'代替target里面的地址,比如我要调用'http://40.00.100.100:3002/user/add'，直接写'/api/user/add'即可
            }
        },
        // 自定义webpack配置
        configureWebpack: config => {
            // 生产环境配置
            if (process.env.NODE_ENV === "production") {
                // 生产环境去除打印日志以及debugger
	        config.optimization.minimizer = [
		    new UglifyJsPlugin({
		        uglifyOptions: {
			    compress: {
				drop_console: true, //console
				drop_debugger: true,
				pure_funcs: ['console.log'] //移除console
			    }
		        }
		    })
	        ]
		//打包文件大小配置
		config["performance"] = {
		    "maxEntrypointSize":10000000,
		    "maxAssetSize":30000000
		}
            } else {
                // 开发环境配置
            }
		}
	}
}
};
```
---
[【精选】vue.config.js配置参考\_vue.config.js pages-CSDN博客](https://blog.csdn.net/weixin_42752574/article/details/116245241)
[Vue项目优化打包——前端加分项 - 掘金](https://juejin.cn/post/7004045635620405278?searchId=202310222029227C396A30ACA858B8E8AE)
## pages
Type: Object
Default: undefined  
在 multi-page 模式下构建应用。每个“page”应该有一个对应的 `JavaScript 入口文件`。其值应该是一个对象，对象的 key 是入口的名字，value 是：**一个指定了 entry, template, filename, title 和 chunks 的对象** (除了 entry 之外都是可选的)； 或**一个指定其 entry 的字符串**。
```js
module.exports = {
  pages: {
    index: {
      // page 的入口
      entry: 'src/index/main.js',
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html',
      // 当使用 title 选项时，
      // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // 在这个页面中包含的块，默认情况下会包含
      // 提取出来的通用 chunk 和 vendor chunk。
      chunks: ['chunk-vendors', 'chunk-common', 'index'],
      outputPath: 'dist/'
    },
    // 当使用只有入口的字符串格式时，
    // 模板会被推导为 `public/subpage.html`
    // 并且如果找不到的话，就回退到 `public/index.html`。
    // 输出文件名会被推导为 `subpage.html`。
    subpage: 'src/subpage/main.js'
  }
}
```
## lintOnSave
- Type: boolean | ‘warning’ | ‘default’ | ‘error’
- Default: ‘default’
是否`在开发环境下通过 eslint-loader 在每次保存时 lint 代码`。这个值会在 `@vue/cli-plugin-eslint` 被安装之后生效。
设置为 `true` 或 `'warning'` 时，`eslint-loader` 会将 `lint 错误输出为编译警告`。
`默认情况下`，警告仅仅会被输出到命令行，且不会使得编译失败。
如果你希望让 `lint 错误`在开发时直接`显示在浏览器中`，你可以使用 `lintOnSave: 'default'`。这会强制 eslint-loader 将 `]lint 错误`输出为编译错误，同时也意味着 lint 错误将会导致`编译失败`。
设置为`error`将会使得 eslint-loader 把 `lint 警告`也输出为编译错误，这意味着 lint 警告将会导致编译失败。

## publicPath
- Type: string
- Default: ‘/’
`默认情况下`，Vue CLI 会假设你的应用是被`部署在一个域名的根路径上`，例如 `https://www.my-app.com`/。如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。  
例如，如果你的应用被部署在 `https://www.my-app.com`/my-app/，则设置 `publicPath 为 /my-app/`。

这个值也可以被设置为`空字符串 ('')` 或是`相对路径 ('./')`，这样所有的资源都会被链接为相对路径，这样打出来的包可以被部署在任意路径，也可以用在类似 Cordova hybrid 应用的文件系统中。
## webpack-chain
[我曾为配置 webpack 感到痛不欲生，直到我遇到了 webpack-chain - 掘金](https://juejin.cn/post/6950076780669566983?searchId=20231022212422189F0FDBB128C9CEE60D)
[前端工程化：webpack-chain - 掘金](https://juejin.cn/post/6844904138954801166?searchId=20231022210431FBE81BB6D00054CC24F8)