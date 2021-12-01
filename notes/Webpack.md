# 入门

## 初始化项目

### 新建目录Webpack，初始化npm。

​	`npm init`

![npm init](C:\Users\10195\Desktop\笔记\images\npm init-webpack.PNG)

​	执行命令后，在Webpack目录下生成文件package.json文件。

​	![初始package.json](C:\Users\10195\Desktop\笔记\images\初始package.json-webpack.PNG)

### 安装两个npm包

​	<b>webpack和webpack-cli</b>

​	`npm i -D webpack webpack-cli`

- npm i -D ：npm install --save-dev的缩写

- npm i -S ：npm install --save的缩写

  此时，在Webpack文件夹下，新增了node_modules文件夹和package-lock.json文件。

### 新建src/main.js

​	新建一个文件夹src，并新建main.js，在文件中编写测试代码

​	`concole.log('Hello webpack');`

### 配置package.json命令

```json
"scripts":{
    "build":"webpack ./src/main.js"
}
```

![配置package.json的build命令](C:\Users\10195\Desktop\笔记\images\配置package.json的build命令-webpack.PNG)

​	此时在Webpack文件夹下生成了一个dist文件夹，并且内部含有main.js，说明打包成功。

![初始打包后的目录结构](C:\Users\10195\Desktop\笔记\images\初始打包后的目录结构-webpack.PNG)

## 自定义配置

上面一个简单的例子只是webpack自己默认的配置，下面我们要实现更加丰富的自定义配置。

### 新建build/webpack.config.js

```js
const path = require('path');
module.exports = {
    mode: 'development', //开发模式
    entry: path.resolve(__dirname, '../src/main.js'), //入口文件
    output: {
        filename: 'output.js',  //打包后的文件名称
        path: path.resolve(__dirname, '../dist') //打包后的目录
    }
}
```

### 更改打包命令

```json
"scripts":{
    "build":"webpack --config build/webpack.config.js"
}
```

执行`npm run build`后，在dist文件夹下生成main.js和output.js

![自定义配置webpack.config.js](C:\Users\10195\Desktop\笔记\images\自定义配置webpack.config.js-webpack.PNG)

## 配置html模板

### 问题

在日常开发时，打包js文件名称不是固定的

```json
module.exports = {
    // 省略其他配置
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    }
}
```

为了解决这个问题，可以使用插件==html-webpack-plugin==解决 ：`npm i -D html-webpack-plugin`

### 新建public/index.html

修改webpack.config.js文件

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode:'development', // 开发模式
    entry: path.resolve(__dirname,'../src/main.js'),    // 入口文件
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    },
    plugins:[
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/index.html')
      })
    ]
}
```

执行`npm run build`命令后，在dist文件下生成index.html文件，其中，打包生成的<b>main.c9d76e6c.js</b>已经被自动引入html文件中

![生成的自动引入script的index.html](C:\Users\10195\Desktop\笔记\images\生成的自动引入script的index.html-webpack.PNG)

此时，项目的目录结构如下所示

![生成index.html后的项目目录结构](C:\Users\10195\Desktop\笔记\images\生成index.html后的项目目录结构-webpack.PNG)

### 多入口文件开发

生成多个html-webpack-plugin实例以实现

```json
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode:'development', // 开发模式
    entry: {
      main:path.resolve(__dirname,'../src/main.js'),
      header:path.resolve(__dirname,'../src/header.js')
  }, 
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    },
    plugins:[
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/index.html'),
        filename:'index.html',
        chunks:['main'] // 与入口文件对应的模块名
      }),
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/header.html'),
        filename:'header.html',
        chunks:['header'] // 与入口文件对应的模块名
      }),
    ]
}
```

执行`npm run build`命令后，在dist文件下生成四个文件index.html和header.html、main.f5a1c634.js和header.f5a1c634.js，其中，js文件已经被自动引入对应的html文件中。

![多文件入口实现目录结构](C:\Users\10195\Desktop\笔记\images\多文件入口实现目录结构-webpack.PNG)

### clean-webpack-plugin

每次执行npm run build 会发现dist文件夹里会残留上次打包的文件，这里我们推荐一个plugin来帮我们在打包输出前清空文件夹`clean-webpack-plugin`

```js
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
module.exports = {
    // ...省略其他配置
    plugins:[new CleanWebpackPlugin()]
}
```

## 引用css

入口文件是main.js，所以在main.js中引入css和less文件

![引入css](C:\Users\10195\Desktop\笔记\images\引入css-webpack.PNG)

安装解析css文件和less构建样式<b>style-loader、css-loader、less和less-loader</b>

修改配置文件webpack.config.js

```js
module.exports = {
    // ...省略其他配置
    module:{
      rules:[
        {
          test:/\.css$/,
          use:['style-loader','css-loader'] // 从右向左解析原则
        },
        {
          test:/\.less$/,
          use:['style-loader','css-loader','less-loader'] // 从右向左解析原则
        }
      ]
    }
} 
```

在文件夹下，打开index.html->F12->Element，可以看到样式文件已经被引入。

![引入css的页面element](C:\Users\10195\Desktop\笔记\images\引入css的页面element-webpack.PNG)

