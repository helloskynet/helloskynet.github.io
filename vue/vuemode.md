# VUE 的 Runtime Only 和 Runtime + Compiler

1. Runtime Only

   Runtime Only 版本，是提前借助其它工具把 .vue 文件编译成 JavaScript，然后才能使用。因为已经提前编译好了，所以它只包含运行时的 Vue.js 代码，因此代码体积也会更轻量。 例如编译器将 .vue 文件编译成 JavaScript 的编译过程中会将组件中的 template 模板编译为 render 函数，所以我们得到的是 render 函数的版本。运行的时候是不需要再次编译的。

2. Runtime + Compiler

   如果没有对代码做预编译，但又使用了 Vue 的 template 属性，则需要在客户端进行编译。

   ```js
   // 需要编译器
   new Vue({ template: "<span></span>" });
   // 不需要编译器
   new Vue({
     render:(h) {
       return h("div", this.hi);
     }
   });
   ```

   &emsp;&emsp;Vue.js 最终渲染都是通过 render 函数。 template 属性最终也会被编译器编译成 render 函数。使用 template 属性的时候就需要引入带编译器的 vue.js，因为带编译器的版本会在浏览器中将 template 转换为 render 函数。在客户的浏览器中进行编译会花费一些时间。

   &emsp;&emsp;所以通常我们更推荐使用 Runtime-Only 的 Vue.js。但是需要我们自己控制 render 函数或者借助工具来编译代码。提前将 template 之类的编译成 render 函数。

   &emsp;&emsp;我们使用 webpack 来构建的时候，就是使用功能 Runtime-Only 版本。vue-loader 完成了编译的任务。
