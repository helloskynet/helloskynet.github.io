# webpack 插件 clean-webpack-plugin

&emsp;&emsp;动态控制生成的文件名之后，每次构建都会生成不同的文件，不会将之前的文件覆盖掉，dist 文件夹下会存在大量的无效文件。每次构建之前需要我们手动将上一次构建的结果删除掉，以保证 dist 文件夹中构建结果是最新的，否则很难区分哪些文件时本次构建生成的，哪些是之前生成的。就需要这个插件来清理上一次的构建结果。

## 安装 clean-webpack-plugin

> npm i clean-webpack-plugin -D

## webpack 中引入使用

配置 webpack.config.js

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

此时再运行 webpack 可以看到 dist 文件夹被清理，只保留本次构建的结果。

## 更多关于 clean-webpack-plugin 的配置

[clean-webpack-plugin 文档](https://github.com/johnagan/clean-webpack-plugin/issues/106)

```js
    // Simulate the removal of files
    // 模拟删除文件   仅仅报告要删除的文件并不删除
    // default: false
    dry: true,

    // Write Logs to Console
    // (Always enabled when dry is true)
    // 报告删除的文件
    // default: false
    verbose: true,

    // Automatically remove all unused webpack assets on rebuild
    // 重建的时候自动删除 webpack 没有使用的资源
    // default: true
    cleanStaleWebpackAssets: false,

    // Do not allow removal of current webpack assets
    // 不允许删除cleanOnceBeforeBuildPatterns 中的文件
    // default: true
    protectWebpackAssets: false,

    // **WARNING**
    //
    // Notes for the below options:
    //
    // They are unsafe...so test initially with dry: true.
    //
    // Relative to webpack's output.path directory.
    // If outside of webpack's output.path directory,
    //    use full path. path.join(process.cwd(), 'build/**/*')
    //
    // These options extend del's pattern matching API.
    // See https://github.com/sindresorhus/del#patterns
    //    for pattern matching documentation

    // Removes files once prior to Webpack compilation
    //   Not included in rebuilds (watch mode)
    // 初始化的时候清理
    // Use !negative patterns to exclude files
    //
    // default: ['**/*']
    cleanOnceBeforeBuildPatterns: ['**/*', '!static-files*'],
    cleanOnceBeforeBuildPatterns: [], // disables cleanOnceBeforeBuildPatterns

    // Removes files after every build (including watch mode) that match this pattern.
    // Used for files that are not created directly by Webpack.
    //
    // Use !negative patterns to exclude files
    // 构建完成后清理
    // default: []
    cleanAfterEveryBuildPatterns: ['static*.*', '!static1.js'],

    // Allow clean patterns outside of process.cwd()
    //
    // requires dry option to be explicitly set
    //
    // default: false
    dangerouslyAllowCleanPatternsOutsideProject: true,
```
