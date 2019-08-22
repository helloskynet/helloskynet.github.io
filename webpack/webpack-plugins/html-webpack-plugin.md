# webpack 的插件 html-webpack-plugin

&emsp;&emsp;文件名的动态生成，防止了缓存的问题，但是也带来了另外一个问题。那就是每次构建结果的文件名都是不同的。我们必须手动修改 html 中的引用才可以继续使用。这是不可以接受的。那我们就要引入 webpack 插件 html-webpack-plugin 来处理这个问题了。由它来更新 html 中对 js 文件的引用。

1. 安装 html-webpack-plugin

   > npm i html-webpack-plugin -D

2. 更新 webpack.config.js

   更新 webpack 的配置文件 webpack.config.js 增加 html-webpack-plugin。

   ```javascript
   // webpack.config.js
   const HtmlWebpackPlugin = require("html-webpack-plugin");

   module.exports = {
     mode: "development",
     entry: {
       main: "./src/index.js"
     },
     output: {
       filename: "[name].[hash].js"
     },
     plugins: [
       new HtmlWebpackPlugin({
         filename: "index.html",
         title: "webpack"
       })
     ]
   };
   ```

3. 使用插件
   此时执行 npx webpack。可以看到输出文件夹 dist 中，会生成一个 index.html 文件，会将构建生成的 js 文件插入到其中。此时 html 使用的是 html-webpack-plugin 自带默认的模板。

   ```html
   <!-- dist文件夹中自动生成的index.html -->
   <!DOCTYPE html>
   <html>
     <head>
       <meta charset="UTF-8" />
       <title>webpack</title>
     </head>
     <body>
       <script
         type="text/javascript"
         src="main.9ac3aa7e8d8de2ef34b1.js"
       ></script>
     </body>
   </html>
   ```

4. 使用 template

   根目录下创建 index.html 作为 template。将 index.html 修改为：

   ```html
   <!-- 根目录下作为template的 index.html -->
   <!DOCTYPE html>
   <html lang="zh-CN">
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <meta http-equiv="X-UA-Compatible" content="ie=edge" />
       <title><%= htmlWebpackPlugin.options.title %></title>
     </head>

     <body>
       <div id="app"></div>
     </body>
   </html>
   ```

   修改 webpack 中 html-webpack-plugin 的配置，增加 template。

   ```javascript
   // webpack.config.js
   ......
     plugins: [
       new HtmlWebpackPlugin({
         filename: "index.html",
         title: "webpack",
         template: './index.html'
       })
     ],
   ......
   ```

运行 webpack 查看 dist 文件夹中生成的结果。

&emsp;&emsp;在模板中我们可以使用`<%= htmlWebpackPlugin.options.title %>`来访问 html-webpack-plugin 配置中的参数，除了默认一些属性之外，可以自定义属性也是可以在模板中通过这种方法访问的，但是尽量不要和官方已有的冲突就好。

&emsp;&emsp;多页面使用 html-webpack-plugin 的时候，每个页面都需要使用 `new HtmlWebpackPlugin()`

## html-webpack-plugin 一些其他配置

[html-webpack-plugin 相关文档](https://webpack.js.org/plugins/html-webpack-plugin/)

[html-webpack-plugin 配置相关文档(github)](https://github.com/jantimon/html-webpack-plugin#options)

1. inject
   注入选项,即指定 js 文件插入到 html 中的位置。有四个选项值 true, body, head, false.
   |value|描述|
   |-|-|
   true|默认值 html 文件的 body 底部
   body|同 true
   head|head 标签内
   false|不插入生成的 js 文件

2. favicon

   给 html 文件指定 favicon 文件所在的路径

3. minify
   minify 的作用是对 html 文件进行压缩，minify 的属性值是一个压缩选项或者 false 。默认值为 false, 不对生成的 html 文件进行压缩。

   webpack mode 为 production 的时候默认值为 true。也可以使用对象对其进行具体配置。

4. hash （Boolean）

   如果为 true 会在插入的文件名末尾增加一串 hash 值。

5. cache

   内容变化的时候重新生成目标文件。

6. showErrors

   把 webpack 的报错插入到当前的 HTML 中，展示到当前的页面。

7. chunks

   指定要引入的块，值为 webpack 配置 entry 属性中的模块名。默认全部引入。

8. excludeChunks

   排除一些块
