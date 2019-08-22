@[toc](vue 开发环境搭建)

# vue 开发环境搭建

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

1. 安装使用 webpack

   [webpack 的安装使用](https://blog.csdn.net/qq_15601471/article/details/89528801)

2. 使用 webpack 插件 html-webpack-plugin

   [html-webpack-plugin 的安装使用](https://blog.csdn.net/qq_15601471/article/details/100011352)

3. clean-webpack-plugin

   &emsp;&emsp;动态控制生成的文件名之后，每次构建都会生成不同的文件，不会将之前的文件覆盖掉，dist 文件夹下会存在大量的无效文件。每次构建之前需要手动将上一次构建的结果删除掉，以保证 dist 文件夹中构建结果是最新的，否则很难区分哪些文件时本次构建生成的，哪些是之前生成的。就需要这个插件来清理上一次的构建结果。

   [clean-webpack-plugin 的安装使用](https://blog.csdn.net/qq_15601471/article/details/100013283)

4. webpack-dev-server

   每次修改完代码，需要手动启动 webpack 打包代码，构建完成后都需要在浏览器中手动刷新，以查看最新的构建结果。所以引入 webpack-dev-server 插件来解决这个问题。

   8.1 安装 webpack-dev-server

   > npm i webpack-dev-server -D

   8.2 webpack.config.js

   webpack-dev-server 插件不需要像其他插件那样去，手动引入。 如果需要修改启动的端口等等再来配置 devServer 属性。目前使用默认值即可，所以暂时不需要修改 webpack.config.js 文件。

   > webpack-dev-server 提供了一个简单的 web 服务器。用于快速开发。  
   > [webpack-dev-server 配置文档](https://www.webpackjs.com/configuration/dev-server/)

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

## 引入 vue

1. 文件准备

   index.js 修改为

   ```javascript
   // index.js
   import Vue from "vue";
   new Vue({
     el: "#app",
     render: h => h()
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

   执行 npm run dev 即可。

   [关于 runtime-only 和 compiler](https://blog.csdn.net/qq_15601471/article/details/100014560)

2) 使用\*.vue 文件 单文件组件

   2.1 添加一个 \*.vue 单文件组件并在 index.js 中使用。

   ```html
   <!-- App.vue -->
   <template>
     <!--  -->
     <div class="container">
       {{ temp }}
     </div>
   </template>

   <script>
     export default {
       data() {
         return {
           temp: "hello world"
         };
       }
     };
   </script>

   <style scoped lang="scss"></style>
   ```

   ```javascript
   // index.js
   import Vue from "vue";
   import App from "./App";

   new Vue({
     el: "#app",
     render: h => h(App)
   });
   ```

   2.1 vue-loader

   &emsp;&emsp;webpack 只能处理 \*.js 和 \*.json 其它类型的文件都需要使用 loader。所以要使用 vue 的单文件组件首先要安装 vue-loader。

   > npm i vue-loader -D

   安装之后，在 webapck 的配置文件 webpack.config.js 中添加匹配规则来使用 vue-loader。

   ```js
   // webpack.config.js
    ...
    module: {
      rules: [
        {
          test: /\.vue$/,
          loader: "vue-loader"
        }
      ]
   },
   ...
   ```

   2.2 vueLoaderPlugin

   &emsp;&emsp;安装之后要在 webpack 的配置里面，添加 vueLoaderPlugin，它的作用基本可以理解为，把 vue-loader 加载进来的文件，将其中的代码分类交给不同的 loader 或者插件处理。例如 script 标签中的代码，交给 js 相关的 loader 以及插件。template 类的交给模板相关的插件。

   ```js
   ...
   const VueLoaderPlugin = require("vue-loader/lib/plugin");
    ...
    plugins: [
    ...
    new VueLoaderPlugin(),
    ...
    ]
    ...

   ```

   如果不引入，运行 webpack 的时候会报如下错误

   ```cmd
    ERROR in ./src/App.vue
    Module Error (from ./node_modules/vue-loader/lib/index.js):
    vue-loader was used without the corresponding plugin. Make sure to include VueLoaderPlugin in your webpack config.
    @ ./src/index.js 2:0-24 6:17-20
   ```

   从错误提示中可以看出，是缺少了 vue-loader 所对应的插件 VueLoaderPlugin 导致的。添加上就可以了。

   2.3 vue-template-compiler

   &emsp;&emsp;之前说过 webpack，只支持 JavaScript 和 json 文件。其余的文件类型都需要相应的插件来处理。vueLoaderPlugin 将 vue 的单文件组件中的代码，分类交给了相应的 webpack 处理程序。webpack 处理了 script 标签中的代码，template 标签中的代码就需要安装 vue-template-compiler 来处理了。

   安装 vue-template-compiler

   `npm i vue-template-compiler -D`

   如果没有安装 vue-template-compiler 就启动 webpack 会报如下错误。

   ```cmd
   ERROR in ./src/App.vue
   Module Error (from ./node_modules/vue-loader/lib/index.js):
   [vue-loader] vue-template-compiler must be installed as a peer dependency, or a compatible compiler implementation must be passed via options.
   @ ./src/index.js 2:0-24 6:17-20
   ```

   从错误中可以看到，必须安装 vue-template-compiler.安装 vue-template-compiler，之后再启动 webpack，就可以正常的编译了。

   2.4 style 相关的 loader

   vue-loader 和它的插件，将 vue 文件中的代码分类，交给了不同的处理程序。template 中的交给了，vue-template-compiler,script 中的交给了 webpack 自身的处理程序。但是没有安装和 style 相关的 loader 为什么没有报错呢，这是因为此时虽然存在 style 标签。但是标签内容是空的。而在 vue 文件组件中 style 并不是必须的。所以没有报错，如果向 style 标签中添加一些代码。再去运行 webpack 就会报错了。

   将 App.vue 修改为：

   ```html
   <!-- App.vue -->
   ....
   <style scoped lang="scss">
     .container {
       background: blue;
     }
   </style>
   ```

   如果此时启动 webapck，报错如下

   ```cmd
   ERROR in ./src/App.vue?vue&type=style&index=0&id=7ba5bd90&scoped=true&lang=scss& (./node_modules/vue-loader/lib??vue-loader-options!./src/App.vue?vue&type=style&index=0&id=7ba5bd90&scoped=true&lang=scss&) 19:0
   Module parse failed: Unexpected token (19:0)
   File was processed with these loaders:
   * ./node_modules/vue-loader/lib/index.js
   You may need an additional loader to handle the result of these loaders.
   ```

   安装插件，

   `npm i style-loader css-loader -D`

   由于在 style 中指定了语言为 scss 所以需要再安装
   `npm i sass-loader node-sass -D`  
   来处理 scss。如果指定了其它语言要安装对应的 loader 即可。

   sass-loader 是将 sass 代码加载进来并调用 node-sass 进行处理，node-sass 将 scss 代码编译成 css 代码。 css-loader 是处理 css 代码中的 url、以及@import 语法等等。style-loader 则通过 style 标签将 css 插入到 DOM 中。

   由上面的每个 loader 的作用判断，在 webpack 配置文件 webpack.config.js 中添加。loader 调用顺序从右至左，从下到上。

   ```js
   ...
   module:{
     rules:[
       ...
      {
        test: /\.scss$/,
        loaders: ["style-loader", "css-loader", "sass-loader"]
      },
      ...
     ]
   }
   ```

   此时在启动 webpack 即可正常编译 style 标签中的内容了。

   2.5 url-loader

   在 css 中使用的图片的时候，需要使用 url-loader 来处理它。

   例如

   ```html
   <!-- App.vue -->
   ...
   <style scoped lang="scss">
     .container {
       height: 100px;
       border: 1px solid blue;
       background: url("./background.jpg");
     }
   </style>
   ```

   安装

   `npm i url-loader -D`

   修改 webpack.config.js

   ```js
   // webpack.config.js
   ...
   module{
     rules;[
       ...
       {
        test: /\.jpg$/,  // 其他的类型按需要添加。例如 /\.(jpg|png|gif)/ 等等
        loader: "url-loader"
       }
     ]
   },
    ...
   ```

## 使用 babel

&emsp;&emsp;webpack 能对 JavaScript 进行处理，只是压缩等操作。如果需要使用 ES6、ES7 等新的语法，就需要使用 babel 了。babel 将其编译后使得不支持 js 新特性的环境也能使用 js 新特性了。

[webapck 中 babel 的配置](https://blog.csdn.net/qq_15601471/article/details/99690530)

## 使用 eslint 和 prettier

&emsp;&emsp;eslint 和 prettier 能够对代码进行语法检测以及代码风格优化。

[使用 eslint 和 prettier](https://blog.csdn.net/qq_15601471/article/details/99985647)
