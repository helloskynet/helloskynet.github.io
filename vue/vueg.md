# vue 开发环境搭建

- vue
- vuex
- webpack

window 下

## 初始化

1. 创建一个文件夹，

   手动在文件管理器里面创建或者通过命令创建，

   > mkdir vuewebpack && cd vuewebpack

   创建 vuewebpack 文件夹 并切换到 vuewebpack 文件夹下面

2. npm 初始化 todo 关于 npm 命令

   > npm init -y

## 安装基本的包

vue、webpack、webpack-cli

1. 安装 vue

   > npm i vue -S

2. 安装 webpack、webpack-cli

   > npm i webpack webpack-cli -D

## 使用 webpack

此时就可以使用 webpack 了。

webpack 的默认入口是 ./src/index.js

1. 创建 src 文件夹，并添加 index.js

   ```javascript
   // index.js
   console.log("webpack");
   ```

2. 使用 webpack

   执行

   > npx webpack

   > 由于我们是局部安装的 webpack，所以不能在命令行中直接 webpack 命令，需要使用 npx 来执行。 也可以在 package.json 添加脚本来调用 webpack。

   [ ] todo 关于 npx

   ```cmd
   //执行命令之后的输出
   F:\vscode\vuewebpack>npx webpack
   Hash: 5523b2c412f96e7e5f05
   Version: webpack 4.39.2
   Time: 790ms
   Built at: 2019-08-15 10:13:16
     Asset       Size  Chunks             Chunk Names
   main.js  952 bytes       0  [emitted]  main
   Entrypoint main = main.js
   [0] ./src/index.js 25 bytes {0} [built]

   WARNING in configuration
   The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
   You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/
   ```

   下面的警告部分是因为我们没有指定，执行环境导致的。警告提示说，默认使用了生产环境。

   > npx webpack --mode development

   指定模式为 development ，指定模式之后即可去除警告。

3. 打包结果

   自动生成的 dist 文件夹下面的 main.js 就是打包之后的结果。

   3.1 我们可以通过在 node 环境中执行代码，测试下打包的结果。

   ```cmd
   F:\vscode\vuewebpack>node dist\main.js
   webpack
   ```

   3.2 也可以新创建一个 index.html 将生成的文件引入到 html 中，然后在浏览器控制台中验证。

   ```html
   <!DOCTYPE html>
    <html lang="zh-CN"></html>
      <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
      </head>

      <body>
        <script src="./dist/main.js"></script>
      </body>
    </html>
   ```

   目录结构图

4. 添加 webpack 配置文件 webpack.config.js

   webpack 相关的配置有很多我们不可能在每次启动的时候都通过，命令行一一传入。所以引入了 webpack.config.js 文件，webpack 默认会在项目根目录所以这个文件，我们也可以在命令行中通过 --config 参数指定配置文件的位置。

   ```javascript
   // webpack.config.js
   module.exports = {
     mode: "development"
   };
   ```

   此时我们就可以直接执行 npx webpack 不用添加 --mode development 参数了。

5. 配置生成文件的文件名。

   生成的文件默认为 main.js，放在 dist 文件夹下面。每次生成文件名不变。使用的时候可能会因为浏览器的缓存造成一些不必要的问题。所以需要动态控制每次生成的文件名，来防止这类问题。

   配置 webpack

   ```javascript
   module.exports = {
     mode: "development",
     entry: {
       main: "./src/index.js"
     }, // 默认值
     output: {
       filename: "[name].[hash].js"
     }
   };
   ```

   [关于 \[name\] 占位符号](https://www.webpackjs.com/configuration/output/#output-filename)

   > 当只有一个入口的时候，可以使用静态的名字加上 hash 值。  
   > 例如：filename: "bundle.[hash].js"。  
   > 如果存在多个入口的时候，要是用 [name] 或者 [chunkhash]。  
   > 例如： filename: "[name].[hash].js"或者 filename: "[name].[chunkhash].js"。  
   > 当然仍可以添加一些静态值，要保证每个入口最终生成的文件名是唯一的。注意 [hash]和[chunkhash]不要同时使用。

   运行 webpack，可以看到生成的文件，添加上了 hash 后缀。文件名变成了：main.f29a0d19150e2033331b.js。
   此时我们再次将它引入 html 文件。就可以继续使用了。

6. html-webpack-plugin

   &emsp;&emsp;文件名的动态生成，防止了缓存的问题，但是也带来了另外一个问题。那就是每次构建结果的文件名都是不同的。我们必须手动修改 html 中的引用才可以继续使用。这是不可以接受的。那我们就要引入 webpack 插件 html-webpack-plugin 来处理这个问题了。由它来更新 html 中对 js 文件的引用。

   6.1 安装 html-webpack-plugin

   > npm i html-webpack-plugin -D

   6.2 更新 webpack.config.js

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

   6.3 此时执行 npx webpack。可以看到输出文件夹 dist 中，会生成一个 index.html 文件，会将构建生成的 js 文件插入到其中。

   ```html
   <!-- dist文件夹中生成的index.html -->
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

   6.4 使用 template

   使用之前在根目录下创建的 index.html 作为 template。将 index.html 修改为：

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

   修改 webpack 的配置

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

   todo 更多关于 html-webpack-plugin 的配置

7. clean-webpack-plugin

   &emsp;&emsp;动态控制生成的文件名之后，每次构建都会生成不同的文件，不会将之前的文件覆盖掉，dist 文件夹下会存在大量的无效文件。每次构建之前需要我们手动将上一次构建的结果删除掉，以保证 dist 文件夹中构建结果是最新的，否则很难区分哪些文件时本次构建生成的，哪些是之前生成的。就需要这个插件来清理上一次的构建结果。

   7.1 安装 clean-webpack-plugin

   > npm i clean-webpack-plugin -D

   7.2 webpack.config.js

   ```javascript
   // webpack.config.js
   const HtmlWebpackPlugin = require("html-webpack-plugin");
   const { CleanWebpackPlugin } = require("clean-webpack-plugin");

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
       }),
       new CleanWebpackPlugin()
     ]
   };
   ```

   7.3 运行 webpack

   执行 npx webpack ，可以看到 dist 文件夹被清空了，只留下了本次的构建结果。

8. webpack-dev-server

   每次修改完代码，需要手动启动 webpack 打包代码，构建完成后都需要在浏览器中手动刷新，以查看最新的构建结果。所以引入 webpack-dev-server 插件来解决这个问题。

   8.1 安装 webpack-dev-server

   > npm i webpack-dev-server -D

   8.2 webpack.config.js

   webpack-dev-server 插件不需要像其他插件那样去，手动引入。 如果需要修改启动的端口等等再来配置 devServer 属性。目前我们使用默认值即可，所以暂时不需要修改 webpack.config.js 文件。

   8.3 运行

   > npx webpack-dev-server

   ```cmd
    F:\vscode\vuewebpack>npx webpack-dev-server
    i ｢wds｣: Project is running at http://localhost:8080/
    i ｢wds｣: webpack output is served from /
    i ｢wds｣: Content not from webpack is served from F:\vscode\vuewebpack
    i ｢wdm｣: Hash: c53ed17adedcaea196f6
    Version: webpack 4.39.2
    Time: 918ms
    Built at: 2019-08-15 14:50:57
    ...
   ```

   打开http://localhost:8080/ 即可看到运行结果。也可以使用--open 参数自动在默认浏览器中打开。

> 不想手动刷新，vscode 中可以试试 Live Server 插件。webpack 的 webpack-dev-server 插件可以视为 watch 模式的 webpack 和 Vscode 的 Live Server 插件的合体。

> webpack-dev-server 插件生成的文件时存储在内存中的，并不会输出。所以看到到 webpack 处理后的文件，如果需要查看文件。可以在浏览器中访问 http://localhost:8080/webpack-dev-server 来查看输出到内存的文件。

todo 更多关于 webpack-dev-server

9. 添加 script 脚本

   运行 webpack 或者 webpack-dev-server 的时候每次都要使用 类似 npx webpack-dev-server，如果有其它的参数则命令会更长。可以把命令放到 package.json 的 script 中去。

   ```json
   // package.json
   ....
     "scripts": {
       "dev": "webpack-dev-server --open",
       "build": "webpack",
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     ....
   ```

   此时我们可以，在命令行执行，npm run dev 即可

## 引入 vue

1. 文件准备

   index.js 修改为

   ```javascript
   // index.js
   import Vue from "vue";

   new Vue({
     el: "#app"
   });
   ```

   在作为模板 index.html 中添加 id 为 app 的 div

   ```html
   <!-- index.html -->
   ...
   <body>
     <div id="app"></div>
   </body>
   ...
   ```

   执行 npm run dev 后，浏览器中查看 index.html。打开控制台会看到如下错误。

   ```cmd
   Navigated to http://127.0.0.1:5501/index.html
   vue.runtime.esm.js:620 [Vue warn]: You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.

   (found in <Root>)
   ...
   vue.runtime.esm.js:8422 You are running Vue in development mode.
   Make sure to turn on production mode when deploying for production.
   See more tips at https://vuejs.org/guide/deployment.html
   ```

   todo 关于 runtime-only 和 compiler

   index.js 修改为

   ```javascript
   // index.js
   import Vue from "vue";
   new Vue({
     el: "#app",
     render: h => h()
   });
   ```

2. 使用\*.vue 文件 单文件组件

   2.1 vue-loader

   &emsp;&emsp;webpack 只能处理 \*.js 和 \*.json 其它类型的文件都需要使用 loader。所以要使用单文件组件首先要，安装 vue-loader。

   > npm i vue-loader -D
