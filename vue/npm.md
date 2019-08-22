# npm 命令

## npm 的使用

npm init 初始化

npm init -y 使用默认值，直接初始化。

npm i webpack -D 等同于 npm install webpack --save-dev
npm i webpack -S 等同于 npm install webpack --save

默认值是-S，但是如果使用 cnpm 必须指定-D 或-S 不能为空。

&emsp;&emsp;--save-dev 安装的包将会存在 package.json 的 devDependencies 属性表明这个模块是在开发过程中依赖的，相当于开发环境中使用的，例如，打包用的 webpack，代码检查的 eslint 等等。

&emsp;&emsp;--save 安装的包将会存在 package.json 的 dependencies 属性上，是生产环境使用的包，例如 jquery，vue 等等。

&emsp;&emsp;安装在这两个地方的包大多数情况是没有什么区别的知识帮助理解，但是如果是 npm 包的话，dependencies 属性中的包会被加入到它的 node_modules 中。

| 参数       | 缩写 | 备注                |
| ---------- | ---- | ------------------- |
| install    | i    |
| uninstall  | uni  | cnpm 不支持这个缩写 |
| --save-dev | -D   |
| -save      | -S   |

## 关于 npx

&emsp;&emsp;npm5.2 开始 加入了 npx 命令，它是 node 自带的模块。

想在命令行下直接调用一个包，有以下几种方法。

1. 全局安装 npm 包，然后就可以直接调用了。

   ```cmd
   >npm i webpack -g // 安装
   >webapck // 调用
   ```

2. 在项目内部安装，使用路径调用

   ```cmd
   >npm i webpack -D  // 安装
   >mode_modules/.bin/webpack 调用
   ```

有了 npx 之后，就可以直接使用 `npx webpack`来调用 webpack 了

命令行 搜索命令顺序 只搜索 系统环境变量 Path 配置

npx 搜索顺序 node_modules/.bin 目录--》 系统环境变量 Path 配置 --》 远程库

npx 在本地搜索不到相关的包的时候会自动搜索远程库，然后下载安装它到一个临时目录使用之后，再删除。

常常使用的 npx 暂时这么多。
