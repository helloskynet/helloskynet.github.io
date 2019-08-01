# VsCode 代码格式化

## 文件格式化

&emsp;&emsp;Ctrl +Alt + L 是格式化当前文件的快捷键，可以手动设置为其它的组合键，但是一般情况没必要修改。  
&emsp;&emsp;在文件中右键之后，点击选择黄色箭头指向的部分，可以手动选择格式化当前文档使用的插件。同时也可以设置默认值，要先安装对应的插件。我是用的是 Prettier - Code formatter 插件。
![文件格式化](https://img-blog.csdnimg.cn/20190801142040368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

&emsp;&emsp;修改 setting.json 文件的时候，输入完属性名，再输入冒号会提示可以输入的值。

![提示默认值](https://img-blog.csdnimg.cn/20190801153912296.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)

## 文件类型关联

&emsp;&emsp;让 VsCode 把指定后缀的文件视为另外一种指定的文件类型处理。方便其调用各种插件完成智能补全、格式化等功能。

```json
 // 文件之间关联，给指定的文件后缀 指定类型方便其去根据类型调用相关插件等等
  "files.associations": {
    "*.tpl": "html",   // 把tpl后缀的文件同样视为html文件处理。
  },
```

## HTML

### 格式化\*.html 文件

设置格式化 html 使用的默认插件，对应的 settings.json 中的配置项为：

```json
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
```

相关配置

```json
{
  "html.format.wrapAttributes": "force", // html 属性强制换行
}
```

### 格式化\*.vue 文件中的 template 标签中的 html 代码

&emsp;&emsp;安装 Vetur 插件。然后在 setting.json 中配置，指定格式化这部分 html 使用的插件。也可不指定，会使用默认值。

```json
{
  "vetur.format.defaultFormatter.html": "js-beautify-html"
}
```

## JavaScript

### 格式化\*.js 文件

设置格式化 JavaScript 使用的默认插件，对应的 settings.json 中的配置项为：

```json
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
```

### 格式化\*.vue 文件中的 script 标签中的 JavaScript 代码

&emsp;&emsp;同样使用 Vetur 插件，setting.json 中的配置为：

默认的就不错，不怎么需要修改，

```json
{
  "vetur.format.defaultFormatter.js": ***,
}
```

相关的配置：

```json
  "vetur.format.defaultFormatterOptions": {
    "prettier": {
      "singleQuote": true,  //javascript 显示为单引号
      "semi": true,         //javascript 行尾加分号
    }
  },
```
