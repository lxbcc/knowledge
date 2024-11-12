[安全验证 - 知乎](https://zhuanlan.zhihu.com/p/628093762)
[webpack系列-面试官：webpack用过么？原理是什么？你做过哪些配置？ - 掘金](https://juejin.cn/post/7140769906080874504?searchId=20231022195645A6838BAE63CD0BBBDD0E#heading-12)
## webpack
Webpack 是一个现代 JavaScript 应用程序的静态模块打包器

### 重要概念
```text
1. entry: 入口文件，指定应用程序的起始点。
2. output: 输出文件，指定打包后的文件名和路径。
3. module：模块配置，用于指定不同类型的文件如何处理。包括各种加载器、解析器等配置。
4. resolve：解析配置，用于指定模块如何被解析。包括别名、扩展名、模块路径等配置。
5. devServer：开发服务器配置，用于指定如何启动和配置开发服务器。包括端口号、代理、热模块替换等配置。
6. mode：模式配置，用于指定Webpack的工作模式。包括开发模式（development）和生产模式（production）。
7. 加载器（Loaders）：Webpack 使用加载器来处理各种类型的文件。加载器可以将文件转换为模块，也可以在转换过程中执行其他任务，例如代码检查、自动添加浏览器前缀等。Webpack 本身只能处理 JavaScript 模块，但是通过使用不同类型的加载器，可以处理各种类型的文件。
8. 插件（Plugins）：插件是一个可以在Webpack构建过程中扩展其功能的 JavaScript对象。Webpack 的很多功能都是通过插件实现的，例如代码压缩、代码分离、代码分析、资源管理等。开发者可以根据自己的需要编写自定义插件，或者使用现成的插件来扩展Webpack的功能。
9. 打包优化（Optimization）：Webpack 提供了多种打包优化的选项，包括代码分离、压缩、懒加载、缓存等。开发者可以通过配置这些选项来提高应用程序的性能和加载速度。
10. 开发模式和生产模式：Webpack 可以通过不同的配置选项来支持开发模式和生产模式。在开发模式下，Webpack 会生成更快的构建结果，并提供更好的开发体验；在生产模式下，Webpack 会生成更小、更快的代码，以便于部署和发布。
11. HMR（Hot Module Replacement）：Webpack 支持热模块替换，即在应用程序运行时，可以动态更新模块的代码，而不需要重新加载整个页面。这可以大大提高开发效率和体验。
```

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
          },
        },
      },
      {
        test: /\.vue$/,
        exclude: /node_modules/,
        use: 'vue-loader',
      },
      {
        test: /\.css$/,
        use: ['vue-style-loader', 'css-loader'],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    port: 8080,
  },
};
```


安装
全局安装
```js
npm install -g webpack
npm install -g webpack-cli
```
项目依赖
```js
npm install --save-dev webpack
npm install --save-dev webpack-cli
```

package.json配置
```js
”scripts": {
    "dev": "webpack --mode development",  // 开发环境
    "build": "webpack --mode production",  // 生产环境
  }

```

webpack.config.js中设置：
```js
module.exports = {
    entry: './src/app.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'main.js'
    },
    mode: 'development' // 设置mode
}
```
development：有内置的调试功能；**打包后的代码可阅读，没被压缩**
production：内置生产阶段的很多优化功能；**代码被压缩**