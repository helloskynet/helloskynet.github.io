@[toc](webpack)

# webpack 的使用（一）

折腾 webpack 好久了，来说说它吧。
![webpack](https://img-blog.csdnimg.cn/20190427220259172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)
&emsp;&emsp;webpack 是一个模块打包器。就是把你的文件打包起来。简单地说就是输入文件然后输出被 webpack 处理后的文件。O(∩_∩)O。

## 初始化

创建一个 webpackDemo 文件夹，执行

```cmd
G:\vscode\webpackDemo>npm init -y
```

可以创建 package.json ，-y 表示使用默认值。

## 安装 webpack

&emsp;&emsp;因为是基于 webpack 4+，webpack-cli 已经独立出来所以需要同时安装 webpack 和 webpack-cli。

```cmd
G:\vscode\webpackDemo>npm i webpack webpack-cli -D
```

> i 是 install 的缩写。-D 是 --save-dev 的缩写

## 执行 webpack

1. 在命令行中

   安装完成之后。如果直接在命令行中执行。

   ```cmd
   G:\vscode\webpackDemo>webpack
   'webpack' 不是内部或外部命令，也不是可运行的程序
   或批处理文件。
   ```

   &emsp;&emsp;这是因为没有全局安装 webpack 和 webpack-cli 的原因。或者也可以说是环境变量配置的问题。

   &emsp;&emsp;这个不重要。因为我们运行 webpack 一般都是通过 package.json 中的 script 运行。直接在命令行里面用的这个真的不多，一般也就刚学的时候测试用用吧。如果需要在命令行中执行 webpack。
   可通过`.\node_modules\.bin\webpack` 指定路径来调用可以了，实在没有全局安装 webpack 的必要。全局安装 webpack 还有可能会引起一些版本问题。

   &emsp;&emsp;此外还可以通过 npx 执行 webpack，，[关于 npx](http://www.ruanyifeng.com/blog/2019/02/npx.html)

   [关于 npx](https://blog.csdn.net/qq_15601471/article/details/99302111)

   ```cmd
   G:\vscode\webpackDemo>npx webpack
   ```

2. 通过 script

   &emsp;&emsp;package.json 的 script 中添加一行 "dev": "webpack"。

   ```json
   {
     "name": "webpackDemo",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "dev": "webpack",
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "keywords": [],
     "author": "",
     "license": "MIT",
     "devDependencies": {
       "webpack": "^4.30.0",
       "webpack-cli": "^3.3.1"
     }
   }
   ```

   然后在命令行中执行`npm run dev`即可。

   ```cmd
   G:\vscode\webpackDemo>npm run dev
   ```

   推荐这种启动方式。

## webpack 的输入、输出和默认值

&emsp;&emsp;此时我们执行 npm run dev 运行的 webpack，是没有传任何参数的，一切使用的都是默认值，命令行中会报错。
![使用默认值执行webpack](https://img-blog.csdnimg.cn/20190425232358624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

> Insufficient number of arguments or no entry found.
> Alternatively, run 'webpack(-cli) --help' for usage info.
> 翻译：
> 参数数量不足或找不到条目。
> 或者，运行“webpack（-cli）--help”获取用法信息。
> ERROR in Entry module not found: Error: Can't resolve './src' in 'G:\vscode\webpackDemo'
> npm ERR! code ELIFECYCLE
> 。。。。。。
> entry 模块未找到，无法处理./src

从报错信息我们可以知道是 webpac 没有找到入口，webpack 默认的入口是“./src/index.js”,
创建相应的目录和文件即可，创建 src 文件夹并在文件夹里面创建 index.js 文件（空文件即可），当然我们也可以通过传入 entry 覆盖默认值，不让 webpack 去找默认位置的文件作为入口。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190425230813526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70#pic_center)

再次执行 npm run dev 来运行 webpack

![执行webpack](https://img-blog.csdnimg.cn/20190425231221157.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

执行成功。黄色部分是警告，大致意思是因为没有指定模式，也就是开发模式还是生产模式的原因，webpack 将其自动设置为 production 模式，如果想要去除这个警告可以在启动的时候增加参数。

```cmd
G:\vscode\webpackDemo>npm run dev -- --mode development
```

将模式指定为开发模式。

![去除警告](https://img-blog.csdnimg.cn/20190425231537371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70#pic_center)

可以看到警告已经消失了。
执行完 npm run dev，可以看到工程目录下多了一个 dist 文件夹，dist 文件夹下有一个 main.js。这就是 webpack 的默认的输出文件位置。"./dist/main.js"。

&emsp;&emsp;我们可以通过--output 参数修改输出的位置。修改 entry 可以通过 --entry 参数修改。接下来我们可以实验一下。
删除 dist 文件夹，将 index.js 文件修改为 index2.js，此时如果执行 npm run dev 肯定会报错，因为 webpack 找不到作为入口的 index.js 文件。
我们使用--entry 参数指定入口，同时使用--output 指定输出位置。

```cmd
G:\vscode\webpackDemo>npm run dev -- --mode development --entry ./src/index2.js --output newoutput/i.js
```

![指定输入输出](https://img-blog.csdnimg.cn/20190425234202285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70#pic_center)

可以看到以 index2.js 为输入文件，i.js 为输出文件成功输出了。

## 通过配置文件配置 webpack

&emsp;&emsp;经过上面的操作我们会发现，当我们需要覆盖默认值的时候要传参数给 webpack，但是在命令行中越写越多。这样当需要修改的参数很多的时候，难度是无法想象的，更别说再不小心写错一个。(╯‵□′)╯︵┻━┻。

&emsp;&emsp;所以 webpack 提供了配置文件，配置文件默认是项目根目录的 webpack.config.js，webpack 运行的时候会自动检测这个文件是否存在，当然了我们可以通过 --config 来指定配置文件，这个等下再说。

接下来在项目根目录添加该文件，

```javascript
// webpack.config.js 使用的是CommonJS规范
const path = require("path"); // 引入的node的路径处理模块

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    // 输出的文件名，可以是一个相对路径，相对于下面的path
    filename: "i.js",
    path: path.resolve("./newoutput")
    // 打包之后文件的就被放在这里。这里必须为绝对路径
    // 默认值为当前项目的dist文件夹文件夹不存在的时候会自动创建
  }
};
```

![配置文件启动webpack](https://img-blog.csdnimg.cn/20190427215849737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70#pic_center)

> 命令行中传递的参数会比配置文件中的参数优先级高。即如果一个参数在命令行和配置文件中同时被指定，会以命令行中的为准。

&emsp;&emsp;npm run dev 此时执行 webpack 就会自动调用 webpack 的配置文件 webpack.config.js，使用里面的配置。

![移动webpack配置文件位置](https://img-blog.csdnimg.cn/20190427222104378.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70#pic_center)

&emsp;&emsp;创建 config 文件夹，将 webpack.config.js 重名为 webpack.js 并移动到 config 文件夹。
此时如果执行 npm run dev 调用 webpack 使用就是默认值了，因为 webpack 在默认位置找不到配置文件了，就会只用默认值了。
使用下面的命令就可继续调用 webpack 的配置文件了。

```cmd
G:\vscode\webpackDemo>npm run dev -- --config ./config/webpack.js
```

我们可以把指定配置文件这部分移动到 package.json 中，package.json 中的 dev 命令修改为

```json
// package.json
{
...,
"scripts": {
    "dev": "webpack --config ./config/webpack.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
}
...
```

这样我们就可以继续使用 npm run dev 启动，而不用再命令行中输入配置文件位置了。

&emsp;&emsp;**这就是 webpack，它根据我们的配置输入指定的文件处理之后再输出到指定的地方。**

> webpack 本身只能打包处理* .js 和 *.json 这两种文件，如果想要处理其它的文件则需要使用 loader 将他们转换成为 webpack 能处理的文件类型之后，webpack 才能处理它们。
