@[toc]

![webpack](https://img-blog.csdnimg.cn/20190427230322963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

将项目文件恢复一下，webpack.config.js 中的内容如图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427230612837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

# webpack 的输入和输入

&emsp;&emsp;webpack 的输入和输出分别有 entry 和 output 控制。

## webpack 的 entry(输入)

&emsp;&emsp;webpack 的输入部分由 entry 来配置，是 webpack 开始构建的起点。
entry 可以是一个字符串，数组，也可以是对象

1. 字符串和数组

   &emsp;&emsp;字符串的时候，就是指定一个文件入口文件的位置。将指定的文件打包到一个文件中。指定为数组的时候，会将数组中的所有文件打包到同一个文件中（也可以说是块）。

   ```javascript
   const path = require("path");

   module.exports = {
     mode: "development",
     // entry: './src/index.js', // 字符串
     entry: ["./src/index.js", "./src/index2.js"], // 数组
     output: {
       filename: "main.js",
       path: path.resolve("./dist")
     }
   };
   ```

2. 对象

   将入口指定为字符串或者数组，实际上是下面的简写。

   ```javascript
   ...
     entry:{
       main: './src/index.js'
     }
   ...
   ```

   可能是单页面比较常用吧，所以给单页面提供了一个简写。

   使用多页面的时候可以指定多个入口，也就是多页面应用程序。每一个入口都会生成一个不同的文件，方便各个页面引用。

   ```javascript
   ...
     entry:{
       pageOne: './src/index.js'，
       pageTwo: './src/index2.js'
       pageThree: ['./src/index.js','./src/index2.js']
     }
   ...
   ```

## webpack 的 output 输出

&emsp;&emsp;webpack 的输出 output 配置必须是一个对象，输出配置只有这一个，即使有多个入口也是一个。

```javascript
  output: {
    filename: 'main.js',
    path: path.resolve('./dist'),
  }
```

filename ，为输出的文件名，

当创建了多个入口的时候。要使用用占位符来确保每个文件具有唯一的名称。

```javascript
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main1: "./src/index.js",
    main2: "./src/index2.js"
  },
  output: {
    filename: "[name].js",
    path: path.resolve("./dist")
  }
};
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428223223929.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

输出的文件为 main1.js 和 main2.js。

| 占位符      | 描述                            |
| ----------- | ------------------------------- |
| [hash]      | 模块 hash                       |
| [chunkhash] | chunk 块的 hash                 |
| [name]      | 模块名称                        |
| [id]        | 模块标示符                      |
| [query]     | 模块的 query 文件名后面的字符串 |

> 当只有一个入口的时候，可以使用静态的名字加上 hash 值。  
> 例如：filename: "bundle.[hash].js"。  
> 如果存在多个入口的时候，要是用 [name] 或者 [chunkhash]。  
> 例如： filename: "[name].[hash].js"或者 filename: "[name].[chunkhash].js"。  
> 当然仍可以添加一些静态值，要保证每个入口最终生成的文件名是唯一的。注意 [hash]和[chunkhash]不要同时使用。

[更多关于占位符号](https://www.webpackjs.com/configuration/output/#output-filename)

慢慢填坑
