@[toc](Babel)

> babel（巴别塔），圣经里面上帝为了阻止人们建造巴别塔（通天之塔），上帝让人类说不同的语言，使人类相互之间不能沟通，计划因此失败，人类自此各散东西。

JavaScript 的 babel 则是为了把各个版本的 JavaScript 翻译成同一种版本或者为它们提供统一的运行环境。

## babel 相关的包

> 7.0 之前版本和之后的版本变化较大，很多包的名字做了修改，为了统一名称，统一修改为@babel 开头，例如 babel-core 修改为@babel/core。

对照表
7.0 之前|7.0 之后|备注
|-|-|-
babel-core|@babel/core
babel-polyfill|@babel/polyfill|7.4 之后弃用，使用其它的代替
babel-runtime|@babel/runtime

---

| 包                              | 简介                                                                                  | 备注                                                                                                                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| babel-loader                    | webpack 的插件加载代码用。                                                            | webpack 使用                                                                                                                                                                       |
| @babel/core                     | babel 的核心，编译器，提供转换的 API。                                                | 转换语法                                                                                                                                                                           |
| core-js                         | 给低版本浏览器提供接口的库。分为 core-js@2 和@core-js@3                               |                                                                                                                                                                                    |
| regenerator-runtime             | 可以理解为：在代码 import 包或者 require 包的时候，为其生成唯一标识防止污染全局变量。 |
| @babel/polyfill                 | 给低版本的浏览器提供接口库 ，本质是由 core-js 和 regenerator-runtime 组成的。         | 7.4 弃用                                                                                                                                                                           |
| @babel/helpers                  | 定义了一些处理新的语法关键字的辅助函数，把需要的辅助函数当做一个模块引入代码          | @babel/core 在处理的时候也会向代码中插入帮助函数，它的处理方法是在每个文件中都插入相应的帮助函数， @babel/helpers 则是把帮助函数当做模块引入，每个文件只需要调用这个模块就可以了。 |
| @babel/plugin-transform-runtime | 引入垫片的时候借助的插件                                                              |                                                                                                                                                                                    |
| @babel/runtime                  | 包含@babel/helpers 和 regenerator-runtime                                             | 功能就是这两个包的组合                                                                                                                                                             |
| @babel/runtime-corejs2          | 由 core-js@2、@babel/helpers 和 regenerator-runtime 组成                              | 功能这三个包的组合                                                                                                                                                                 |
| @babel/runtime-corejs3          | 由 core-js@3、@babel/helpers 和 regenerator-runtime 组成                              | 功能这三个包的组合                                                                                                                                                                 |
| @babel/preset-env               | 插件集预设配置，                                                                      |
| @babel/preset-stage-X           | 插件集，包含 stage-0,1,2,3                                                            | V7.0 后 弃用，这些插件集中语法特性的并不一定能够确定进入最终的标准，因为有人可能会无限制的在生产环境中使用它们导致一些问题，所以 babel 官方弃用了它们。                            |

> 关于插件集@babel/preset-env：我们在进行语言转换给浏览器提供垫片（polyfill）的时候，有几种选择，
>
> 1. 全部引入，即把所有的垫片全部引入，这样有些功能我们可能就没有使用，而引入它们的垫片白白增加了大量的无效代码。
> 2. 手动按需引用，比如当前文件用到了什么，就在文件中引入什么。例如我们文件中使用了 Promise，就在文件中引入，`import '@babel/runtime-corejs3/core-js/promise'`，这样带来的问题是，如果多个文件使用了它，每个文件都要引入。
> 3. 使用 webpack 打包，我们可以把所有需要的垫片收集起来，统一在入口文件中引入，这样就只需要引入一次即可。但是如果垫片很多的话，在入口文件里面就会出现一排排的，重复相似代码
>
> ```js
> import "@babel/runtime-corejs3/core-js/promise";
> import "@babel/runtime-corejs3/core-js/xxx";
> import "@babel/runtime-corejs3/core-js/xxx";
> import "@babel/runtime-corejs3/core-js/xxx";
> ```
>
> 为了处理这样的代码，所以引入了插件集。插件集会根据配置自动转换语法、引入的垫片，就不需要在入口处放置一大堆声明了。

## 在 webpack 中使用

1. 安装相关的包

   `npm i @babel/core @babel/preset-env babel-loader -D`

2. 配置 webpack

```javascript
// webpack.config.js
  module: {
    rules: [
      ...
        {
          test: /\.js$/,
          exclude: /node_moduels/,
          use: {
            loader: "babel-loader"
          }
        }
      ],
  }
....
```

3. 配置 babel

   1. babel.config.js（官方推荐）
   2. .babelrc
   3. package.json 中创建 "babel" 属性
   4. .babelrc.js
   5. 使用 webpack 的 loader 的配置。

这几种方法都可以用来配置 babel，至于优先级自己试试吧，应该没有人同时用好几个的。

[官方相关配置文档](https://babeljs.io/docs/en/configuration)

根据官方的建议。如果想以编程的方式创建配置，需要编译 node_modules 中的内容那就使用 babel.config.js,如果只是一个简单的包的静态配置就使用，.babelrc，

就当前情况暂时先选择，.babelrc 来配置。

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "chrome": "68",
          "ie": "11"
        }
      }
    ]
  ]
}
```

## 使用方式

### polyfill 方式

babel-polyfill 已经被废弃，但是 core-js 和它的作用类似。

全部引入`import "core-js/stable";`，也可以按照需要使用什么引入什么。

安装 babel 相关的包

```cmd
npm i @babel/core @babel/preset-env babel-loader -D
```

配置文件

```json
// .babelrc
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "chrome": "68",
          "ie": 10
        },
        "useBuiltIns": "entry",
        "corejs": 2
      }
    ]
  ]
}
```

参数介绍，

target： 转换结果的目标，代码最终的执行环境，根据目标环境判断是否需要对语法进行转换，对那些语法进行转换例如箭头函数、class 语法等等。

useBuiltIns: false | "entry" | "usage"

| 选项             | 描述                                                                         |
| ---------------- | ---------------------------------------------------------------------------- |
| false （默认值） | 此时 corejs 属性无效，不引入垫片，仅仅根据 target 属性判断后转换相应的语法， |
| "entry"          | 在入口处手动引入垫片，                                                       |
| "usage"          | 自动检测文件中使用的需要引入垫片的地方并自动引入对应的垫片。（官方推荐）     |

corejs: 2| 3

corejs 属性仅仅在 useBuiltIns 设置为 非 false 的时候有效。

corejs 设置不同参数的时候注意安装对应的包。如果 useBuiltIns 为 "entry" 的时候还需要手动在入口引入。

| corejs 选项 | 需要额外安装的包   |
| ----------- | ------------------ |
| 2(默认值)   | npm i core-js@2 -S |
| 3           | npm i core-js@3 -S |

2 和 3 的区别有点闹不明白。

### transform-runtime 方式使用

runtime-transform 和 core-js 类似，但是 transform-runtime 提供的沙箱式的垫片单独生成了一个运行沙箱，不会污染全局变量。一般用在当代码运行在我们不可控的环境中的时候，例如；发布的 npm 包

安装相关包

`npm i @babel/core @babel/preset-env @babel/plugin-transform-runtime babel-loader -D`

配置文件

```json
// .babelrc
{
  "presets": [["@babel/preset-env"]],
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "corejs": 3
      }
    ]
  ]
}
```

corejs 设置不同参数的时候需要安装对应的包。

| corejs 选项   | 描述                                                                  | 需要额外安装的包                |
| ------------- | --------------------------------------------------------------------- | ------------------------------- |
| false(默认值) | 只辅助转换语法，如：箭头函数 、class 语法                             | npm i @babel/runtime -S         |
| 2             | 在 false 的基础上增加支持全局变量和静态方法，例如 Promise，Array.from | npm i @babel/runtime-corejs2 -S |
| 3             | 在 2 的基础上增加支持实例方法 如 [].includes()                        | npm i @babel/runtime-corejs3 -S |

两种使用方式的区别在于是否污染全局环境。不能同时使用。

## 在 VUE 中的使用

&emsp;&emsp;在 VUE 中推荐使用 polyfill 的方式。因为以 transform-runtime 方式使用的时候，默认情况下只会转换我们的代码，不会给 node_module 中的库添加垫片什么的，如果想让它转换 mode_module 中的代码还需要使用 babel.config.js 来特殊配置，而且转换的结果还不一定能够使用。node_module 中的库可能会自带垫片，但是如果你想使用功能 vuex, 浏览器控制台就会报错，提示需要我们提供全局的垫片，因为 vuex 没有自带垫片。

&emsp;&emsp;我们自己开发 web 程序，目前最省事就是进行如下配置，把所有的 polyfill 引入这样也就不用担心缺少什么东西了。到了后面有时间如果想优化打包，比如减少打包结果的体积什么的，可以再进行优化。前期开发直接全部引入，不要用什么再引入什么不然来回修改显得有点麻烦。

要安装的包`npm i @babel/core @babel/preset-env babel-loader -D`和`npm i core-js@3 -S`;

在文件入口 index.js 开头引入 core-js

```js
// index.js
import 'core-js/stable`
```

或者也可以在 webpack 打包配置中增加 core-js;

```js
// webpack.config.js
...
module.exports = {
  ...
  entry: {
    main: ["core-js/stable", "./src/index.js"]
  },
  ...
}

```

babel 的配置文件

```json
// .babelrc
{
  "presets": [
    [
      "@babel/preset-env",
      {
        // target 要根据自己的情况进行配置
        "targets": {
          "chrome": "68",
          "ie": 10
        },
        "useBuiltIns": "entry",
        "corejs": 3
      }
    ]
  ]
}
```
