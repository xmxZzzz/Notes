# html

## 1.body中，引入的文件是按照==顺序执行==的。

```html
<body>
    <div id="app" v-cloak>
        <input-number v-model="initValue" :max="10" :min="0" :step="3"></input-number>
    </div>
    <!-- script文件顺序引入 -->
    <script src="https://unpkg.com/vue/dist/vue.min.js"></script>
    <script src="input-number.js"></script>
    <script src="index.js"></script>
</body>
```

## 2.HTML5新特性data_*

- HTML5规范里增加了一个自定义data属性。
- 这个自定义data属性的用法非常的简单，就是可以往 HTML 标签上添加任意以"==data-=="开头的属性， **这些属性页面上是不显示的，它不会影响到你的页面布局和风格，但它却是可读可写的。**
- 使用 **data-** 可以解决自定义属性混乱无管理的现状。

## 3. colgroup标签

- `<colgroup>` 标签用于对表格中的列进行组合，以便对其进行格式化。

- 通过使用`<colgroup>`标签，可以向整个列应用样式，而不需要重复为每个单元格或每一行设置样式。

- 只能在` <table>` 元素之内，在任何一个 `<caption> `元素之后，在任何一个`<thead>`、`<tbody>`、`<tfoot>`、`<tr>` 元素之前使用 `<colgroup> `标签。

- 示例：

  ```html
  <table border="1">
    <colgroup>
      <col span="2" style="background-color:red;">
      <col style="background-color:yellow" width="150px" >
    </colgroup>
    <tr>
      <th>ISBN</th>
      <th>Title</th>
      <th>Price</th>
    </tr>
    <tr>
      <td>3476896</td>
      <td>My first HTML</td>
      <td>$53</td>
    </tr>
  </table>
  ```

  ![colgroup标签示例](C:\Users\10195\Desktop\笔记\Notes\images\colgroup.png)

  【注】：因为现在主流的浏览器Firefox、Chrome 以及 Safari 仅支持 colgroup 元素的 span 和 width 属性。

# JavaScript

## 1.contains

<b>contains方法是用来判断元素A是否包含了元素B，包含返回true，不包含返回false。</b>

```html
<div id="parent">
    <div id="children">
    </div>
</div>

<script type='text/javascript'>
    var parent = document.getElementById("parent");
    var children = document.getElementById("children");
    console.log(parent.contains(children)); //true
    console.log(children.contains(parent)); //false
</script>
```

## 2.支持键盘的上下键实现功能

==@keyup.up== 和 ==@keyup.down==

```html
<div id="app" v-cloak>
     <input type="text" :value="currentValue" @change="handleChange" @keyup.up="handleIncrease" @keyup.down="handleReduce">
</div>
```

## 3.Array.apply()

- 用于创建数组，接收两个参数， 第一个为调用时指定的上下文（context）； 第二个为一个数组或者一个类数组对象；
- 与`Array()`和`new Array()`的区别
  - `Array()`和`new Array()`：创建的数组是一个有length属性的空数组，其中的每个元素**还没有被赋值（初始化)**。也就是`JavaScript`会自动为数组的每一项赋值 `undefined` ，而这个`undefined`相当于一个占位符。
  - `Array.apply(null, {length: 20})`：以这种方式创建出来的数组，数组中的每一项一创建出来就被赋上了确确实实的值`undefined `，即**被初始化为`undifined`**。
- `Array.apply(null,{length:5}).map(function(){return createElement(Child);})`
  - 首选创建5个初始值为null的数组
  - 然后遍历该数组，并将每个值设置为render函数生成的组件

## 4. ES6

### `export`和`import`

- 用来导出和导入模块。
- 一个模块就是一个js文件，它拥有独立的作用域，里面定义的变量外部是无法获取的。

```js
// config .js 
var Config = { 
	version :’1.0.0’ 
}
export { Config }; 

或：

// config .js 
export var Config = { 
	version :’1.0.0’ 
}

// add .js 
export function add(a , b) { 
	return a + b;
}

// main.js 
import { Config } from ’./config.js’; 
import { add } from ’./add.js’;
console.log(Config) ; // { version :’1.0.0’ } 
console.log(add(1,1)); // 2
```

### `export default`

- 当用户不想去了解名称是什么，指示把模块的功能拿来使用，或者想自定义名称，可以使用`export default`来输出默认的模块

```js
// config.js 
export default { 
	version :’1.0.0’ 
}
// add . js 
export default function (a, b) { 
	return a + b; 
}
// main.js
import conf from ’./config.js’; 
import Add from ’./add.js’;
console.log(Config) ; // { version :’1.0.0’ } 
console.log(add(1,1)); // 2
```

- 使用**npm**安装的库，在webpack中可以直接导入

  ```js
  import Vue from 'vue';
  import $ from 'jquery';
  ```

### 箭头函数

- 箭头函数里的*this*指向与普通函数是不一样的，即定义时所在的对象，而不是使用时所在的对象。

  ```js
  function Timer(){
      this.id=1;
      var _this = this;
      setTimeout(function(){
          console.log(this.id); //undefined
          console.log(_this.id); //1
      },1000);
      
      setTimeout(()=>{
          console.log(this.id); //1
      },1000);
  }
  
  var timer = new Timer();
  ```

  

# CSS3

## 1.滑动动画

​	![滑动动画](C:\Users\10195\Desktop\笔记\Notes\images\滑动动画.gif)

- 使用`<transition></transition>`包裹需要滑动的组件，并指定 <b>name</b>和 <b>mode</b>

  ```vue
  <transition name="fade" mode="out-in">
      <div class="pane" v-show="show">
          <slot></slot>
      </div>
  </transition>
  ```

- 设置滑动动画

  ```css
  .pane{
      display: inline-block;
  }
  /*入场(离场)动画的时间段   */
   .fade-enter-active,.fade-leave-active{
       position: absolute;
       transition: all .8s ease;  /*ease:快启动，慢停止，物理原则*/
  }
  /*v-enter 是进入之前，元素的起始状态*/
  /*v-leave-to 离开之后动画的终止状态*/
  .fade-enter, .fade-leave-to{
      transform: translateX(100px); /*X轴平移*/
      opacity: 0;  /*透明度*/
  }
  ```

- [Vue.js](https://cn.vuejs.org/v2/api/#transition)官网中存在对==transition==的详细说明

## 2.[基本样式](https://www.runoob.com/css/css-tutorial.html)

### 2.1 display

| 值                 | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| none               | 此元素不会被显示。                                           |
| block              | 此元素将显示为块级元素，此元素前后会带有换行符。             |
| inline             | 默认。此元素会被显示为内联元素，元素前后没有换行符。         |
| inline-block       | 行内块元素。（CSS2.1 新增的值）                              |
| list-item          | 此元素会作为列表显示。                                       |
| run-in             | 此元素会根据上下文作为块级元素或内联元素显示。               |
| compact            | CSS 中有值 compact，不过由于缺乏广泛支持，已经从 CSS2.1 中删除。 |
| marker             | CSS 中有值 marker，不过由于缺乏广泛支持，已经从 CSS2.1 中删除。 |
| table              | 此元素会作为块级表格来显示（类似 <table>），表格前后带有换行符。 |
| inline-table       | 此元素会作为内联表格来显示（类似 <table>），表格前后没有换行符。 |
| table-row-group    | 此元素会作为一个或多个行的分组来显示（类似 <tbody>）。       |
| table-header-group | 此元素会作为一个或多个行的分组来显示（类似 <thead>）。       |
| table-footer-group | 此元素会作为一个或多个行的分组来显示（类似 <tfoot>）。       |
| table-row          | 此元素会作为一个表格行显示（类似 <tr>）。                    |
| table-column-group | 此元素会作为一个或多个列的分组来显示（类似 <colgroup>）。    |
| table-column       | 此元素会作为一个单元格列显示（类似 <col>）                   |
| table-cell         | 此元素会作为一个表格单元格显示（类似 <td> 和 <th>）          |
| table-caption      | 此元素会作为一个表格标题显示（类似 <caption>）               |
| inherit            | 规定应该从父元素继承 display 属性的值。                      |

### 2.2 border

  - 缩写边框属性设置在一个声明中所有的边框属性。
  - 可以设置的属性分别（按顺序）：*border-width*, *border-style* 和 *border-color*。

### 2.3 padding

  - padding 简写属性在一个声明中设置所有填充属性。该属性可以有1到4个值：==上、右、下、左==

  - 实例

    ```css
    padding:10px 5px 15px 20px;
        上填充是 10px
        右填充是 5px
        下填充是 15px
        左填充是 20px
    
    padding:10px 5px 15px;
        上填充是 10px
        右填充和左填充是 5px
        下填充是 15px
    
    padding:10px 5px;
        上填充和下填充是 10px
        右填充和左填充是 5px
    
    padding:10px;
    	所有四个填充都是 10px
    ```

### 2.4 border-radius

  -  这个属性允许为元素添加**圆角边框**。
  - border-radius 属性是一个最多可指定四个 border -*- radius 属性的复合属性
  - 每个半径的四个值的顺序是：==左上角，右上角，右下角，左下角==。如果省略左下角，右上角是相同的。如果省略右下角，左上角是相同的。如果省略右上角，左上角是相同的。

### 2.5 cursor

  - 定义了鼠标指针放在一个元素边界范围内时所用的**光标形状**

  | 值        | 描述                                                         |
  | :-------- | :----------------------------------------------------------- |
  | *url*     | 需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
  | default   | 默认光标（通常是一个箭头）                                   |
  | auto      | 默认。浏览器设置的光标。                                     |
  | crosshair | 光标呈现为十字线。                                           |
  | pointer   | 光标呈现为指示链接的指针（一只手）                           |
  | move      | 此光标指示某对象可被移动。                                   |
  | e-resize  | 此光标指示矩形框的边缘可被向右（东）移动。                   |
  | ne-resize | 此光标指示矩形框的边缘可被向上及向右移动（北/东）。          |
  | nw-resize | 此光标指示矩形框的边缘可被向上及向左移动（北/西）。          |
  | n-resize  | 此光标指示矩形框的边缘可被向上（北）移动。                   |
  | se-resize | 此光标指示矩形框的边缘可被向下及向右移动（南/东）。          |
  | sw-resize | 此光标指示矩形框的边缘可被向下及向左移动（南/西）。          |
  | s-resize  | 此光标指示矩形框的边缘可被向下移动（南）。                   |
  | w-resize  | 此光标指示矩形框的边缘可被向左移动（西）。                   |
  | text      | 此光标指示文本。                                             |
  | wait      | 此光标指示程序正忙（通常是一只表或沙漏）。                   |
  | help      | 此光标指示可用的帮助（通常是一个问号或一个气球）。           |

### 2.6 outline

  - outline（轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

  - 可以设置的属性分别是（按顺序）：==outline-color, outline-style, outline-width==

    - **outline-style**的属性值

      | 值      | 描述                                                |
      | :------ | :-------------------------------------------------- |
      | none    | 默认。定义无轮廓。                                  |
      | dotted  | 定义点状的轮廓。                                    |
      | dashed  | 定义虚线轮廓。                                      |
      | solid   | 定义实线轮廓。                                      |
      | double  | 定义双线轮廓。双线的宽度等同于 outline-width 的值。 |
      | groove  | 定义 3D 凹槽轮廓。此效果取决于 outline-color 值。   |
      | ridge   | 定义 3D 凸槽轮廓。此效果取决于 outline-color 值。   |
      | inset   | 定义 3D 凹边轮廓。此效果取决于 outline-color 值。   |
      | outset  | 定义 3D 凸边轮廓。此效果取决于 outline-color 值。   |
      | inherit | 规定应该从父元素继承轮廓样式的设置。                |

### 2.7 position

  | 值                                                           | 描述                                                         |
  | :----------------------------------------------------------- | :----------------------------------------------------------- |
  | [absolute](https://www.runoob.com/css/css-positioning.html#position-absolute) | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
  | [fixed](https://www.runoob.com/css/css-positioning.html#position-fixed) | 生成固定定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
  | [relative](https://www.runoob.com/css/css-positioning.html#position-relative) | 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
  | [static](https://www.runoob.com/css/css-positioning.html#position-static) | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
  | [sticky](https://www.runoob.com/css/css-positioning.html#position-sticky) | 粘性定位，该定位基于用户滚动的位置。它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。 |
  | inherit                                                      | 规定应该从父元素继承 position 属性的值。                     |
  | initial                                                      | 设置该属性为默认值，详情查看 [CSS initial 关键字](https://www.runoob.com/cssref/css-initial.html)。 |

### 2.8 box-shadow

- 设置一个或多个下拉阴影的框。

- box-shadow: *h-shadow v-shadow blur spread color* inset

  | 值         | 说明                                                         |
  | :--------- | :----------------------------------------------------------- |
  | *h-shadow* | 必需的。水平阴影的位置。允许负值                             |
  | *v-shadow* | 必需的。垂直阴影的位置。允许负值                             |
  | *blur*     | 可选。模糊距离                                               |
  | *spread*   | 可选。阴影的大小                                             |
  | *color*    | 可选。阴影的颜色。在[CSS颜色值](https://www.runoob.com/cssref/css_colors_legal.aspx)寻找颜色值的完整列表 |
  | inset      | 可选。从外层的阴影（开始时）改变阴影内侧阴影                 |

# Vue

## 1.v-model

`v-model`就是`prop:value`和`event:input`组合使用的一个语法糖。

## 2.自定义指令

### 变量声明

- 使用`el.__xxx__`在上下文中声明一个变量，不能使用`this.xxx`
- 在`bind()`方法中添加监听方法，还需要在`unbind()`方法中移除该监听，使用中间变量`el.__xxx__`

## 3.Virtual Dom

`Virtual Dom`并不是真正意义上的DOM，而是一个轻量级的**JavaScript对象**。

### 运行过程

![Virtual Dom运行过程](C:\Users\10195\Desktop\笔记\Notes\images\Virtual Dom运行过程-16387802008143.PNG)

### VNode

- 在Vue.js2中，Virtual Dom就是通过一种**VNode**类表达的，每个DOM元素或组件都对应一个VNode对象。 
- -在Vue.js源码中是这样定义的：

```js
export interface VNode { 
    tag?: string;  //标签名
    data?: VNodeData;  //当前节点的数据对象
    children?: VNode[];  //子节点，数据
    text?: string; //当前节点的文本
    elm?: Node;  //当前虚拟节点对应的真实DOM节点
    ns?: string; //节点的namespace
    context?: Vue;  //节点上下文，编译的作用域
    key?: string I number;  //节点的key属性
    componentOptions?: VNodeComponentOptions;  //创建组件实例时会用到的选项信息
    componentInstance?: Vue; //组件实例
    parent?: VNode ; // 当前节点的父节点
    raw?: boolean;  //是否为原生HTML或只是普通文本
    isStatic?: boolean; //静态节点的标识 keep-alive
    isRootinsert: boolean;  //是否作为根节点插入，被<transition>包裹的节点，该属性的值为false
    isComment: boolean; //当前节点是否为注释节点 
}
```

- 其中`data`的类型`VNodeData`的代码如下:

  ```js
  export interface VNodeData { 
      key? : string I number; 
      slot?: string; 
      scopedSlots? : { [key: string] : ScopedSlot ) ; 
      ref? : string; 
      tag? : string; 
      staticClass?: string; 
      class? : any ; 
      staticStyle?: { [key: string] : any }; 
      style?: Object[] I Object ; 
      props?: {[key: string] : any} ; 
      attrs?: { [key: string] : any }; 
      domProps? : {[key : string] : any} ; 
      hook?: { [key: string] : Function }; 
      on?: { [key: string]: Function I Function[] }; 
      nativeOn? : { [key: string] : Function I Function[] }; 
      transition?: Object ; 
      show?: boolean ; 
      inlineTemplate?: { 
          render : Function; 
          staticRenderFns : Function[]
  	}; 
      directives?: VNodeDirective[] ; 
      keepAlive? : boolean ;
  }

### Render函数

**Render**函数通过 ==createElement== 参数来创建 **Virtual Dom** ，结构精简了很多。

### createElement

- 构成了`Vue Virtual Dom`的模板，它包含3个参数

  ```js
  createElement(
  	//第一个参数：{ Stirng | Object | Function }
      	//一个HTML标签，组件选项或一个函数，必须 return 上述其中一个
      ’div‘,
      
      //第二个参数：{Object}
      	//一个对应属性的数据对象，可以在template中使用，可选项
      {
          // 详细
      },
      
      //第三个参数：{String | Array}
      	//子节点(VNode)，可选项
      [
          //第一个参数为div，第三个参数为hello world
          createElement('div',"hello world"),
          
          //第一个参数为自定义组件MyComponent，第二个参数为数据对象，包含的props为someProp
          createElement(MyComponent,{
          	props:{
          		someProp:'foo'	
          	}
          }),
          //第三个字节点为一个字符串bar
      	'bar'
      ]
  )
  ```

- 对于事件修饰符和按键修饰符，基本也需要自己实现。

  - 表格
  
    | 修饰符                     | 对应的句柄                                                   | 说明                                                         |
    | -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | .stop                      | event.stopPropagation()                                      | 阻止冒泡行为,不让当前元素的事件继续往外触发,如阻止点击div内部事件,触发div事件 |
    | .prevent                   | event.preventDefault()                                       | 阻止事件本身行为,如阻止超链接的点击跳转,form表单的点击提交   |
    | .self                      | if(event.target!==event.currentTarget) return                | 只有是自己触发的自己才会执行,如果接受到内部的冒泡事件传递信号触发,会忽略掉这个信号 |
    | .enter、.13                | if(event.keyCode!=13) return 替换13位需要的keyCode           |                                                              |
    | .ctrl、.alt、.shift、.meta | if(!event.ctrlKey) return 根据需要替换ctrlKey为altKey、shiftKey或metaKey |                                                              |
  
  - 特殊修饰符: **`.capture`**和**`.once`**
  
    | 修饰符                         | 前缀 | 说明                                                         |
    | :----------------------------- | ---- | ------------------------------------------------------------ |
    | .capture                       | !    | 改变js默认的事件机制,默认是冒泡,capture功能是将冒泡改为倾听模式 |
    | .once                          | ~    | 将事件设置为只执行一次,如 .click.prevent.once 代表只阻止事件的默认行为一次,当第二次触发的时候事件本身的行为会执行 |
    | .capture.once 或 .once.capture | ~!   |                                                              |
  
    - 写法 
  
      ```js
      on:{
          '!click':this.doThisInCapturingMode,
          '~keyup':this.doThisOnce,
          '~!mouseover':this.doThisOnceInCapturingMode
      }
      ```
  

## 4.插件

1. 注册插件需要一个公开的方法*install*，它的第一个参数是Vue构造器，第二个参数是一个可选的选项对象。

```js
MyPlugin.install=function(Vue,option){
	//全局注册组件（指令等公共资源类似）
    Vue.component('componnet-name',{
        //组件内容
    })
    //添加实例方法
    Vue.prototype.$Notice=function(){
        //逻辑
    }
    //添加全局方法或属性
    Vue.globalMethod=function(){
        //逻辑
    }
    //添加全局混合
    Vue.mixin({
        mounted:function(){
            //逻辑
        }
    })
}	
```



### 路由



# Webpack

## 1.概述

### `webpack`是前端工程化模块打包工具。

### 模块化示意图

![webpack模块化示意图](C:\Users\10195\Desktop\笔记\Notes\images\webpack模块化示意图-16387802710314.png)	

### 主要应用场景是：**单页面富应用（SPA）**

- *SPA*：全称`Single Page Application`，通常是由一个`html`文件和一堆按需加载的`js`组成。

## 2.安装webpack与wabpack-dev-server

### 步骤

1. 新建目录demo
2. npm init
3. 执行包含一系列选项，最终获得一个package.json文件
4. 本地局部安装webpack

​		`npm install webpack@2.3.2 --save-dev`

4. 本地局部安装webpack-dev-server

​		`npm install webpack-dev-server@2.4.2 --save-dev`

5. 最终的pakeage.json文件

![安装webpack后的package.json文件](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\安装webpack后的package.json文件.png)

【注】此处webpack和webpack-dev-server的版本号较低，高版本无法正常执行。

### 配置

1. 新建**webpack.config.js**文件

```js
var config = {

};

// 相当于export default config;
// 但因为还未安装支持ES6的编译插件，所以不能直接使用ES6的语法
module.exports = config;
```

2. 在package.json文件中配置快速启动webpack-dev-server服务的脚本

```js
 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --host 172.172.172.1 ---port 8888 --open --config webpack.config.js"
  },
```

其中，

- `--config`：指向*webpack-dev-server*读取的配置文件路径，即*webpack.config.js*的路径
- `--open`：在执行命令时自动在浏览器打开页面，默认地址是 **127.0.0.1:8080**。
- `--host`：配置IP
- `--port`：配置端口

3. **入口（Entry）和出口（Output）**

- 入口：告诉webpack从哪里开始寻找依赖，并且编译
- 出口：用来配置编译后的文件存储位置和文件名

```js
var path = require('path');

var config = {
    //入口
    entry: {
        main: './main.js'
    },
    //出口：打包后的文件会存储为demo/dist/main.js文件，只要在html中引入它就可以了
    output: {
        path: path.join(__dirname, './dist'),  //存放打包后文件的输出目录
        publicPath: '/dist/',	//指定资源文件引用的目录
        filename: 'main.js' //指定输出文件的名称
    }
};

// 相当于export default config;
// 但因为还未安装支持ES6的编译插件，所以不能直接使用ES6的语法
module.exports = config;
```

4. 执行 `npm run dev`

![webpack初次运行的浏览器结果](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\webpack初次运行的浏览器结果.png)

5. 当在*main.js*文件中添加语句`document.getElementById("app").innerHTML = "Hello webpack.";`后，浏览器自动刷新，即通过建立一个**WebSocket**连接来实时响应代码的修改。

![webpack-dev-server热更新](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\webpack-dev-server热更新.png)

6. **WebSocket**

- 一种在单个[TCP](https://baike.baidu.com/item/TCP)连接上进行[全双工](https://baike.baidu.com/item/全双工)通信的协议。WebSocket通信协议于2011年被[IETF](https://baike.baidu.com/item/IETF)定为标准RFC 6455，并由RFC7936补充规范。WebSocket [API](https://baike.baidu.com/item/API)也被[W3C](https://baike.baidu.com/item/W3C)定为标准。
- WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

### 更多配置

- **加载器**

  对于不同模块（.css、.js、.html、.jpg、.less等），需要用不同的加载器（Loader）来处理。通过安装不同的加载器可以对各种后缀名的文件进行处理。

  - CSS样式，就需要用到style-loader和css-loader

    ```shell
    npm install style-loader@0.23.1 --save-dev
    npm install css-loader@2.0.2 --save-dev
    ```

    【注】此处webpack和webpack-dev-server的版本号较低，高版本无法正常执行。

  - 在webpack.config.js文件中配置加载器

    ```js
    var path = require('path');
    
    var config = {
       //...
        //配置加载器Loader
        //增加对.css文件的处理
        modules: {
            rules: [
                {
                    test: /\.css/,
                    use: [
                        'style-loader',
                        'css-loader'
                    ]
                }
            ]
        }
    };
    
    module.exports = config;
    ```

    - `rules`中用于指定一系列加载器loader，每一个loader都必须包含`test`和`use`两个选项。
    - 配置内容的意思是：当webpack编译过程中遇到`require()`或`import`语句导入一个后缀名为`.css`的文件时，先将它通过`css-loader`转换，再通过`style-loader`转换，然后继续打包。
    - `use`选项的值可以时数组或字符串，如果是数组，它的编译顺序就是**从后向前**。

  - 再次执行`npm run dev`
  - ![CSS加载器处理后的结果](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\CSS加载器处理后的结果.png)
    - CSS是通过Javascript动态创建`<style>`标签来写入的，意味着样式代码都已经编译在了main.js文件中。

- **插件Plugins**

  - 当项目很大样式就会很多，都放在JS里太占体积，还不能做缓存，因此需要用到**插件Plugins**。

  - 使用**extract-text-webpack-plugin**插件将散落在各地的css提取出来，并生成main.css文件，最终在index.html中通过`<link>`的形式加载它。

    - `npm install extract-text-webpack-plugin@2.1.2 --save-dev`

    - 在webpack.config.js配置文件中导入插件，并改写loader的配置

      ```js
      //导入插件
      var ExtractTextPlugin = require('extract-text-webpack-plugin');
      
      var config = {
       	//...
          module: {
              rules: [
                  {
                      test: /\.css/,
                      // use: [
                      //     'style-loader',
                      //     'css-loader'
                      // ]
                      //利用插件改写use
                      use: ExtractTextPlugin.extract({
                          use: 'css-loader',
                          fallback: 'style-loader'
                      })
                  }
              ]
          },
          plugins: [
              //重命名提取后的css文件
              new ExtractTextPlugin('main.css')
          ]
      };
      
      module.exports = config;
      ```

    - 在index.html中通过`<link>`标签引入

      ```html
      <head>
          <meta charset="UTF-8">
          <meta http-equiv="X-UA-Compatible" content="IE=edge">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>webpack app</title>
          <link rel="stylesheet" type="text/css" href="/dist/main.css">
      </head>
      ```

    - 结果：<link>`引入的main.css文件替换`<style>`

      ![使用插件管理CSS文件后的结果](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\使用插件管理CSS文件后的结果.png)

## 3. 单文件组件与vue-loader

### 概述

- `Vue.js`是一个渐进式的*JavaScript*框架，在使用*webpack*构建*Vue*项目时，可以使用一种新的构建模式：***.vue*单文件组件**。
- *.vue*单文件组件就是一个后缀名为*.vue*的文件，在*webpack*中使用**vue-loader**就可以对*.vue*格式的文件进行处理。
- 一个*.vue*文件一般包含3部分：<template>、<script>、<style>

### 配置

1. 安装依赖

   ```shell
   npm install --save vue
   npm install --save-dev vue-loader 
   npm install --save-dev vue-style-loader 
   npm install --save-dev vue-template-compiler //.vue 文件模版解析器。
   npm install --save-dev vue-hot-reload-api //热更新
   npm install --save-dev babel  //Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码
   npm install --save-dev babel-loader 
   npm install --save-dev babel-core  //babel转译器本身，提供了babel的转译API，如babel.transform等，用于对代码进行转译。
   npm install --save-dev babel-plugin-transform-runtime //babel转译过程中使用到的插件
   npm install --save-dev babel-preset-es2015  //可以将es6的代码编译成es5,transform阶段使用到的一系列的plugin
   npm install --save-dev babel-runtime //它是将es6编译成es5去执行。功能类似babel-polyfill，一般用于library或plugin中，因为它不会污染全局作用域
   ```

   

2. 修改配置文件*webpack.config.js*来支持对*.vue*文件及ES6的解析

   ```js
   var path = require('path');
   //导入插件
   var ExtractTextPlugin = require('extract-text-webpack-plugin');
   
   var config = {
       //...
       module: {
           rules: [
               {
                   test: /\.vue/,
                   loader: 'vue-loader',
                   options: {
                       loaders: {
                           css: ExtractTextPlugin.extract({
                               use: 'css-loader',
                               fallback: 'vue-style-loader'
                           })
                       }
                   }
               },
               {
                   test: '/\.js/',
                   loader: 'babel-loader',
                   //排除node_modules文件夹
                   exclude: /node-modules/  
               },
               {
                   test: /\.css/,
                   // use: [
                   //     'style-loader',
                   //     'css-loader'
                   // ]
                   //利用插件改写use
                   use: ExtractTextPlugin.extract({
                       use: 'css-loader',
                       fallback: 'style-loader'
                   })
               }
           ]
       },
       plugins: [
           //重命名提取后的css文件
           new ExtractTextPlugin('main.css')
       ]
   };
   
   module.exports = config;
   ```

- *vue-loader*在编译*.vue*文件时，会对<template>、<script>、<style>分别处理，所以在*vue-loader*选项里多了一个*options*来进一步对不同语言进行配置。
- 还可以指定不同的语言，比如`<template lang="jade">`、`<style lang="less">`，然后配置相应的加载器就可以。

3. 新建*.babelrc*文件，并写入*babel*配置，*webpack*会依赖此配置文件来使用*babel*编译*ES6*代码

   ```json
   {
   	//预设：presets属性告诉Babel要转换的源码使用了哪些新的语法特性，presets是一组Plugins的集合。    
       "presets": [
           "es2015"
       ],
       //插件
       "plugins": [
           //解决全局对象或者全局对象方法编译不足的情况
           "transform-runtime"
       ],
       //在生成的文件中，不产生注释
       "comments": false
   }	
   ```

### `url-loader`和`file-loader`

- webpack.config.js文件中进行配置

  ```js
  {
      test: /\.(gif|jpg|png|woff|svg|eot|ttf)\??.*$/,
      //如果文件小于1kb,则不会生成一个文件
      loader: 'url-loader?limit=1024' 
  }
  ```

### 用于生产环境

- **《Vue.js实战》**所介绍和使用的都是单页面富应用（SPA）技术，这意味着最终稿只有一个html的文件，其余都是静态资源。
- 实际部署到生产环境时，一般会将*html*挂在后端程序下，由后端路由渲染这个页面，将所有的静态资源*（css、js、image、iconfont等）*单独部署到*[CDN](https://baike.baidu.com/item/CDN/420951)*，当然也可以和后端程序部署在一起，这样就实现了前后端完全分离。

- **ejs**是一个**JavaScript**模板库，用来从*JSON*数据中生成*HTML*字符串，常用于*Node.js*。
