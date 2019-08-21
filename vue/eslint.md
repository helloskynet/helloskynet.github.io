# vscode 下使用 eslint 和 vscode 进行代码格式化以及检查

## 关于 eslint 和 prettier

[eslint 文档](https://eslint.org/)

[prettier 文档](https://prettier.io/)

### eslint

&emsp;&emsp;eslint 主要用来检查语法问题，例如：声明一个变量应该 const 还是 let ，使用的变量有没有声明等等。

&emsp;&emsp;eslint 也可以用来检测代码风格问题，但是没有 prettier 做的好。

### prettier

&emsp;&emsp;prettier 主要用来检查代码风格问题，它支持多种语言，我们这里讨论关于 JavaScript 的，例如应该使用单引号还是双引号，该不该换行，tab 键占多少个空格，结尾要不要分号 等等问题。

&emsp;&emsp;使用 prettier 来处理代码风格的时候，就不要再同时使用 eslint 来处理代码风格问题了，两者同时使用可能会产生冲突，造成一些不必要的麻烦。

## 在 vscode 组合使用 eslint 和 prettier

### 使用 eslint 检查语法，并 eslint 中调用 prettier 来格式化代码风格

1. 安装 eslint 以及 prettier

   `npm i eslint prettier -D`

2. 初始化 eslint

   `npx eslint --init`

   ```cmd
   ? How would you like to use ESLint? To check syntax and find problems
   ? What type of modules does your project use? JavaScript modules (import/export)
   ? Which framework does your project use? Vue.js
   ? Where does your code run? (Press <space> to select, <a> to toggle all, <i> to invert selection)Browser
   ? What format do you want your config file to be in? JavaScript
   ```

   回答完问题之后，会生成配置文件 .eslintrc.js 同时会自动安装相关的包，如果安装失败了可以手动安装。

   > 生成的配置文件会根据最后一个问题的选择不同而生成不同的文件。如： .eslintrc.js，.eslintrc，.eslintrc.json 等等

3. 配置 prettier

   在根目录添加 .prettierrc.js 即可 prettier 会自动搜索相关的配置文件。

   > 配置文件有多种，如：.prettierrc.json，.prettierrc.js 等等。选择一种即可，不同的配置文件写法不同。

   ```js
   // .prettierrc.js
   module.exports = {
     tabWidth: 2, // tab 占据两个空格
     semi: true // 语句结尾的分号
   };
   ```

   也可以增加一些其他规则，具体配置详见 [prettier 的配置](https://prettier.io/docs/en/options.html)

4. 配置 eslint 使其调用 prettier

   修改 eslint 的配置文件，使它能够调用 prettier。需要安装`eslint-plugin-prettier`。 `npm i eslint-plugin-prettier -D`。

   ```js
   // .eslintrc.js
   module.exports = {
     env: {
       browser: true,
       es6: true,
       node: true
     },
     extends: ["eslint:recommended", "plugin:vue/essential"],
     globals: {
       Atomics: "readonly",
       SharedArrayBuffer: "readonly"
     },
     parserOptions: {
       ecmaVersion: 2018,
       sourceType: "module"
     },
     plugins: ["vue", "prettier"], // 这里增加prettier插件。
     rules: {
       "prettier/prettier": "error" // prettier 检测到的标红展示
     }
   };
   ```

   [eslint 的配置](https://eslint.org/docs/user-guide/configuring)

5. 使用 eslint

   5.1 命令行中使用

   &emsp;&emsp;此时我们在命令行中运行 `npx eslint "src\index.js"` 就会输出该文件相关的错误，如果有的错误可以自动修复会在下方输出提示。这个时候使用`npx eslint --fix "src\index.js"` 可以自动修复这一部分的错误。

   > 也可以指定文件夹进行操作， 如 npx eslint "src";

   ```

    F:\vscode\vuewebpack>npx eslint "src\index.js"

    F:\vscode\vuewebpack\src\index.js
      27:7   error  'reg' is assigned a value but never used  no-unused-vars
      27:13  error  Spaces are hard to count. Use {4}         no-regex-spaces
      27:21  error  Insert `;`                                prettier/prettier

    ✖ 3 problems (3 errors, 0 warnings)
      2 errors and 0 warnings potentially fixable with the `--fix` option.

    F:\vscode\vuewebpack>npx eslint --fix "src\index.js"

    F:\vscode\vuewebpack\src\index.js
      27:7  error  'reg' is assigned a value but never used  no-unused-vars

    ✖ 1 problems (1 errors, 0 warnings)

   ```

   5.2 通过 vscode 的 eslint 插件使用

   &emsp;&emsp;在 vscode 中搜索并安装 eslint 插件，eslint 插件会将 eslint 检测到的错误代码下面标出红线以便识别。在 vscode 的配置文件 setting.json 中增加配置。

   ```json
   // setting.json
   ...
   "eslint.autoFixOnSave": true,  // 保存时自动修复
   "eslint.validate": [  // 验证的文件类型。
     "javascript",
     "javascriptreact",
     "html",
     {
       "language": "typescript",
       "autoFix": true
     },
     {
       "language": "vue",
       "autoFix": true
     }
   ],
   ...
   ```

   此时当文件保存的时候，vscode 的 eslint 插件就会自动将能够修复的错误修复。

### 使用 prettier 来格式化代码风格，并 prettier 中调用 eslint 检查语法

> 前面三个步骤和之前的相同。

1. 安装 eslint 以及 prettier

---

2. 初始化 eslint

---

3. 配置 prettier

---

4.  prettier 调用 eslint

    prettier 并不能直接调用 eslint，在 vscode 中可以通过插件 Prettier - Code formatter，配置`"prettier.eslintIntegration":true`这个属性来调用 eslint,

    但目前总是报类似下面的这种错误。

        Failed to load plugin 'standard' declared in 'CLIOptions'f:\vscode\vuewebpack\src\index.js:: Cannot find module 'eslint-plugin-standard'
        Require stack:

        - D:\Microsoft VS Code\_\_placeholder\_\_.js

    一直找不到原因猜测可能是和下面这个问题有关 eslint5 到 eslint6 做了比较大的改动。

    > 官方文档中  
    > WARNING: Due to a bug in the prettier-eslint library, this extension is NOT compatible with ESLint version 6.

5.  prettier 的使用

    1. 命令行调用

       ```cmd
       // 检测代码是否有格式问题
       F:\vscode\vuewebpack>npx prettier -c "src\index.js"
       Checking formatting...
       src\index.js
       Code style issues found in the above file(s). Forgot to run Prettier?
       // 修复问题
       F:\vscode\vuewebpack>npx prettier --write "src\index.js"
       src\index.js 73ms
       ```

    2. vscode 中通过 Prettier - Code formatter 插件调用

       &emsp;&emsp;下载并安装 vscode 的 Prettier - Code formatter 插件。在 vscode 配置文件 setting.json 中增加如下配置，设置 JavaScript 语言的默认格式化程序为 prettier，然后使用
       <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>L</kbd>即可格式化文件。

       ```json
       "[javascript]": {
         "editor.defaultFormatter": "esbenp.prettier-vscode"
       },
       ```

       > 其他的语言设置类似

## eslint 的使用

1. 忽略规则

   1. 添加 .eslintignore 文件

      在项目根目录添加 .eslintignore 在文件添加上需要忽略的文件或者文件夹即可。

      ```json
      // .eslintignore
      webpack.config.js
      src
      ```

   2. 在代码中忽略规则

      在代码中可以通过注释来控制一些规则的生效与否

      ```js
      debugger; // 报错
      var b = 1; // 报错
      /* eslint-disable */
      debugger; // 不报错
      var a = 1; // 不报错
      /* eslint-enable */
      debugger; // 报错
      var b = 1; // 报错
      /* eslint-disable no-debugger */
      debugger; // 不报错
      var c = 1; // 报错
      ```

      > eslint-disable,eslint-enable 都可以指定一些规则的生效与否，多个规则的时候用逗号分隔。不指定规则的时候默认全部规则。

   3. 在配置文件中关闭规则

      配置文件中的 rules 属性就是用来控制各种规则开启以及关闭，设置报错的级别等等。

      ```js
        // .eslintrc.js
        ...
          rules: {
          "no-console": "off", // eslint6 中将 no-console这个规则移出了推荐规则。
        }
        ...
      ```

      > "off" or 0 - turn the rule off  
      > "warn" or 1 - turn the rule on as a warning (doesn’t affect exit code)  
      > "error" or 2 - turn the rule on as an error (exit code is 1 when triggered)

      [eslint 的配置](https://eslint.org/docs/user-guide/configuring)

2. 全局变量

   &emsp;&emsp;js 中我们使用一些全局变量的时候 eslint 可能提示，变量未定义之类的错误。我们可以注释或者配置告诉 eslint 那些变量是全局变量。

   1. 通过注释

      ```js
      b = 1; // 不报错
      /* global b:writeable  */
      b = 1; // 不报错
      ```

      默认情况下全局变量是只读的，如果我们只配置`/* global b*/` 此时 eslint 则会提示全局变量是只读的不可以赋值。所以这里增加了一个参数 writeable。如果没有给全局变量赋值，只是取值则不需要增加这个参数。

   2. 通过配置文件

      配置文件中的 globals 属性就是用来配置全局变量的。

      ```js
      // .eslintrc.js
      ...
      globals: {
        a: "readonly",
        b: "writeable",
      },
      ...
      ```

3. 自定义 eslint 规则

   1. 使用配置包

      eslint 中我们可以使用别人已经定义好的规则集（配置包）。而不用一个一个的再 rules 中添加规则。

      ```js
      // .eslintrc.js
      ...
      extends: "eslint:recommended", // 使用官方推荐配置
      ...
      ```

      使用其它的配置包的时候，首先需要安装配置包，配置包的命名规则是 eslint-config-\*，例如 eslint-config-standard 以及 eslint-config-vue，安装之后，在配置增加即可，在 extends 属性中可以省略前缀 eslint-config-。

      ```js
      // .eslintrc.js
      ...
      extends: ["eslint:recommended","standard"],  // 引入额外的配置包
      ...
      ```

   2. 使用插件配置

      插件则是 以 eslint-plugin-开头。安装相关插件后，在 plugins 中引入，然后在 rules 中声明之后就可以使用了，也可以 extends 中增加配置批量使用插件中的规则。 例如 eslint-plugin-vue，

      ```js
      // .eslintrc.js
        extends: ["plugin:vue/essential"],
        plugins: ["vue"],
      ```

   3. 自定义规则

      如果要自定义规则我们首先要通过 plugins 插件创建一个规则。

      这个需要点 AST 的知识，后面再补吧。

## 关于 eslint 的使用

&emsp;&emsp;想要在 webpack 中使用还需要安装 eslint-loader 但是不推荐在 webpack 构建过程中进行 eslint 代码检查，会导致的构建时间大大延长。

&emsp;&emsp;eslint 的检查应该在之前的步骤中进行处理，例如：通过插件在编辑器中实时检测问题并修复。也可通过工具在 git 提交的时候进行检查。

&emsp;&emsp;在 package.json 增加脚本 `"lint":"eslint src"`，可以随时用来检查自己工程中的问题，并及时修复。vscode 等等的插件只会检测工作区的问题，对于没有打开的文件不会提示 eslint 错误。
