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


### 循环

#### for... in

<u>*es5*</u>标准，用来**遍历key值**，遍历对象和数组。

```js
// 遍历对象
let obj = {
  a: 1,
  b: 2,
  c: 3
}
for (let key in obj) {
  console.log(key)
}  // a  b  c

// 遍历数组
let arr = [1, 2, 3]
for (let key in arr) {
  console.log(key)
}  // 0  1  2
```

#### for...of

<u>*es6*</u>标准，用来**遍历value值**，遍历数组，不能遍历普通对象[^1]。

```js
// 遍历数组
let arr = [1, 2, 3]
for (let value of arr) {
  console.log(value)
}  // 1  2  3
```

## substr()

- substr() 方法可在字符串中抽取从 *start* 下标开始的指定数目的字符。<u>（左包含，右不包含）</u>

- 语法

  ```js
  stringObject.substr(start,length)
  ```

  | 参数     | 描述                                                         |
  | :------- | :----------------------------------------------------------- |
  | *start*  | 必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。 |
  | *length* | 可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 *stringObject* 的开始位置到结尾的字串。 |

## native修饰符

```vue
 <Item @click.native="handleClick(item.id)"></Item>
```

- 当不加`.native`修饰符时，默认是监听来自自定义组件*Item*组件的<u>**自定义事件**</u>。
- 当加上`.native`修饰符，监听的是点击Item组件时触发的**<u>原生click事件</u>**。

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

[aaaa](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

### 2.9 overflow

- 规定当内容溢出元素框时发生的事情

- 常用值

  | 值      | 描述                                                     |
  | :------ | :------------------------------------------------------- |
  | visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
  | hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
  | scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
  | auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
  | inherit | 规定应该从父元素继承 overflow 属性的值。                 |

### 2.10 text-decoration

- text-decoration 属性规定添加到文本的修饰。

- 可能的值

  | 值           | 描述                                            |
  | :----------- | :---------------------------------------------- |
  | none         | 默认。定义标准的文本。                          |
  | underline    | 定义文本下的一条线。                            |
  | overline     | 定义文本上的一条线。                            |
  | line-through | 定义穿过文本下的一条线。                        |
  | blink        | 定义闪烁的文本。                                |
  | inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |

# Ajax、jQuery和Axios

## Ajax简介

- 全称：Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）
- 描述一种使用现有技术集合的"新"方法，包括：<u>*HTML 或 XHTML、CSS、JavaScript、DOM、XML、XSLT， 以及最重要的 **XMLHttpRequest***</u>。

- 最大的<u>优点</u>是：==在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。==

- 不需要任何浏览器插件，但需要用户允许*JavaScript*在浏览器上执行。

## jQuery简介

- jQuery是一个快速、简洁的 *JavaScript框架*，是继[Prototype](https://baike.baidu.com/item/Prototype/14335188)之后又一个优秀的JavaScript代码库（框架）于2006年1月由[John Resig](https://baike.baidu.com/item/John Resig/6336344)发布。
- jQuery设计的宗旨是==“write Less，Do More”==，即倡导写更少的代码，做更多的事情。它封装*JavaScript*常用的功能代码，提供一种简便的 <u>JavaScript设计模式</u>，优化*HTML*文档操作、事件处理、动画设计和*Ajax*交互。
- jQuery的**<u>核心特性</u>**可以总结为：
  - 具有独特的链式语法和短小清晰的多功能接口；
  - 具有高效灵活的CSS选择器，并且可对CSS选择器进行扩展；
  - 拥有便捷的插件扩展机制和丰富的插件。
- jQuery兼容各种主流浏览器，如IE 6.0+、FF 1.5+、Safari 2.0+、Opera 9.0+等。

## Axios简介

- Axios 是一个基于 ***promise*** 的*HTTP库*，可以用在浏览器和 node.js 中。
- 特性
  - 从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
  - 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
  - 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
  - 拦截请求和响应
  - 转换请求数据和响应数据
  - 取消请求
  - 自动转换 JSON 数据
  - 客户端支持防御 [XSRF

## jQuery和Axios对Ajax的封装的区别

### Ajax

```js
//创建异步对象  
var xhr = new XMLHttpRequest();
//设置请求基本信息，并加上请求头
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xhr.open('post', 'test.php' );
//发送请求
xhr.send('name=Lan&age=18');
xhr.onreadystatechange = function () {
    // 这步为判断服务器是否正确响应
    if (xhr.readyState == 4 && xhr.status == 200) {
        console.log(xhr.responseText);
    } 
};
```

- **优点**：局部更新；原生支持
- **缺点**：可能破坏浏览器后退功能；嵌套回调
- 详细示例可参考下文的[Vue-插件](#ajax)

### jQuery

实现了对ajax技术的封装，但是jQuery主要是对原生JavaScript进行封装，封装了js三大核心要素：ECMAScript、DOM、BOM，所以说jQuery封装的ajax只是其中的一小部分，如果通过引用jQuery来进行ajax交互实在是显得有所浪费资源。

```js
$.ajax({
   type: 'POST',
   url: url,
   data: data,
   dataType: dataType,
   success: function () {},
   error: function () {}
});
```

- **<u>缺点</u>**

  - 本身是针对MVC的编程,不符合现在前端MVVM的浪潮

  - 基于原生的XHR开发，XHR本身的架构不清晰，已经有了fetch的替代方案

  - JQuery整个项目太大，单纯使用ajax却要引入整个JQuery非常的不合理（采取个性化打包的方案又不能享受CDN服务）

### axios

#### 简介

通过promise技术实现对ajax实现的一种封装，符合最新的ES规范。本身上来说axios就是ajax，但是ajax却不单单只是axios。

#### 示例

```js
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```

#### axios常用语法

| nam                               | value                               |
| --------------------------------- | ----------------------------------- |
| axios(config)                     | 通用/最本质的发任意类型请求的方式   |
| axios(url[,config])               | 可以只指定url发get请求              |
| axios.request(config)             | 等同于axios(config)                 |
| axios.get(url[,config])           | 发get请求                           |
| axios.delete(url[,config])        | 发delete请求                        |
| axios.post(url[,data,config])     | 发post请求                          |
| axios.put(url[,data,config])      | 发put请求                           |
| axios.defaults.xxx                | 请求非默认全局配置                  |
| axios.interceptors.request.use()  | 添加请求拦截器                      |
| axios.interceptors.response.use() | 添加响应拦截器                      |
| axios.create([config])            | 创建一个新的axios(他没有下面的功能) |
| axios.Cancel()                    | 用于创建取消请求的错误对象          |
| axios.CancelToken()               | 用于创建取消请求的token对象         |
| axios.isCancel()                  | 是否是一个取消请求的错误            |
| axios.all(promise)                | 用于批量执行多个异步请求            |

【补充说明】

- `axios.create(config)`： 对axios请求进行二次封装
  - 根据指定配置创建一个新的 axios ,也就是每个axios 都有自己的配置
  - 新的 axios 只是没有 **取消请求** 和 **批量请求** 的方法，其它所有语法都是一致的
  - 为什么要这种语法？
    - <u>需求</u>，项目中有部分接口需要的配置与另一部分接口的配置不太一样
    - <u>解决</u>：创建2个新的 axios ，每个都有自己的配置，分别对应不同要求的接口请求中

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

2. 通过`Vue.use(MyPlugin)`或`Vue.use(MyPlugin,{ //参数} )`来使用插件。

- 绝大多数情况下，开发插件主要是通过*NPM*发布后给别人使用。在自己的项目中，可以直接在入口调用以上的方法，无需多以不注册和使用的步骤。

### 前端路由与vue-router

#### 概述

- *SPA*的核心就是**路由**。

  - 通俗地说，就是*网址*，比如https://www.baidu.com
  - 专业地说，就是每次GET或POST等请求在服务端有一个专门的正则配置列表，然后匹配到具体的一条路径后，分发到不同的*Controller*，进行各种操作，最终将*html*或数据返回给前端，这就晚会蹭了一次*IO*。

- 目前绝大多数的网站都是*<u>后端路由</u>*，也就是多页面的。

  - <u>优点</u>：页面可以在服务端渲染后直接返回给浏览器，不用等待前端加载任何*js*和*css*就可以直接显示网页内容；对[搜索引擎优化SEO](https://baike.baidu.com/item/%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E4%BC%98%E5%8C%96/3132?fr=aladdin)的友好等。
  - <u>缺点</u>：模板是由后端来维护或改写的。前端开发者需要安装整套的后端服务，必要时还得学习像*PHP*或*Java*这些非前端语言来改写*html*结构，所以*html*和数据、逻辑混为一谈，维护起来既臃肿又麻烦。

- 针对上述缺点，也就有了==前后端分离的开发模式==，后端只提供*API*来返回数据，前端通过*ajax*来获取数据后，再用一定的方式渲染到页面里。

  - <u>优点</u>：前后端做的事情分得很清楚，后端专注在数据上，前端专注再交互和可视化上。
  - <u>缺点</u>：首屏[^2]渲染需要时间来加载*css*和*js*。

  <u>*SPA*就是在前后端分离的基础上，加一层前端路由。</u>

#### 前端路由

即由前端来维护一个路由规则。

##### 有两种实现方式

- 一种是利用*url*的*hash*，就是常说的<u>锚点（#）</u>。*JavaScript*通过*hashChange*事件来监听*url*的变化，*IE7*及以下需要用轮询。	
- 另一种就是==*HTML5*的*History*模式==。它使url看起来像普通网站那样，以”/“分割，没有”#“，但页面并没有跳转，不过使用这种模式需要服务端支持，服务端在接收到所有的请求后，都执行同一个html文件，不然会出现404。因此，**SPA只有一个*html***，整个网站所有的内容都在这一个*html*里，通过*JavaScript*来处理。

##### 优点

- 页面持久性：例如在播放音乐的同时跳转到另一个页面，而音乐没有中断
- 前端后彻底分离

##### 框架

- 通用的框架：Director
- 具体的框架：*Angular*的*ngRouter*，*React*的*ReactRouter*，以及==*Vue*的*vue-router*==。

#### vue-router

##### 基本用法

*<b>vue-router</b>*的实现原理与`<component :is="currentCompnent"></component>`的通过 *<u>is</u>* 特性来实现动态组件类似。

1. 通过*npm*安装*vue-router*

   `npm install vue-router@2.3.1`

2. 在*main.js*文件中使用`Vue.use()`加载插件

   ```js
   import VueRouter from 'vue-router';
   Vue.use(VueRouter);
   ```

3. 针对*views/index.vue*和*views/about.vue*两个页面，在*main.js*文件中配置对应的**路由匹配列表**

   ```js
   //创建数组用来制定路由匹配列表，每一个路由映射一个组件
   const Routers = [
       {
           //path: 指定当前匹配路径
           path: '/index',
           //component: 映射的组件
           //异步实现的懒加载,好处是不需要在打开首页的时候就把所有的页面内容全部加载进去，只在访问时才加载
           component: (resolve) => require(['./views/index.vue'], resolve)
           //如果非要一次性加载
           //component:require('./views/index.vue')
       },
       {
           path: '/about',
           component: (resolve) => require(['./views/about.vue'], resolve)
       },
        //动态路由，配置动态id
       {
           path: '/user/:id',
           component: (resolve) => require(['./views/user.vue'], resolve)
       },
       //当访问的路径不存在时，重定向到首页
       {
           path: '*',
           redirect: '/index'
       }
   ]
   ```

   - **webpack会把Routers中的每一个路由都打包为一个js文件**，js文件又被叫作*<u>chunk</u>*（块），它们的<u>命名方式默认是0.main.js、1.main.js......</u>。可以在webpack配置的出口output处通过设置**chunkFilename**字段修改chunk命名。

     ```
     output:{
     	publicPath:'/dist/',
     	filename:'[name].js',
     	chunkFilename:'[name].chunk.js'
     }
     ```

     在请求到该页面时，才加载这个页面的js，也就是异步实现的懒加载（按需加载）。

   - **动态路由**：路由的一部分是固定的，一部分是动态的：`/user/123456`，其中用户*id* "123456"就是动态的，但它们路由到同一个页面。

     ```vue
     <template>
         <div>
             {{$route.params.id}}
         </div>    
     </template>
     <script>
     export default {
         mounted(){
             //this.$route可以访问到当前路由的信息
             console.log(this.$route.params.id);
         }
     }
     </script>
     <style scoped>
     
     </style>
     ```

     - 打开[127.0.0.1:8080/user/123456](127.0.0.1:8080/user/123456)，如图所示。

       ![动态路由](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\动态路由.png)

4. 完成路由配置和路由实例

   ```js
   const RouterConfig = {
       //使用HTML5的History路由模式
       mode: 'history',
       routes: Routers
   }
   const router = new VueRouter(RouterConfig);
   
   new Vue({
       //...
       router: router,
       //...
   })
   ```

   - 不配置 *mode* 或者 `mode:'hash'` ，就会使用”==#==“来设置路径。
   - 使用HTML5的History路由模式，通过”==/==“设置路径。开启*History*路由，在生产环境时服务端必须进行配置，将所有路由都指向同一个*html*，或设置404页面为该*html*，否则刷新时页面会出现404。

5. *webpack-dev-server*也要配置下来支持*History*路由，在package.json文件中修改dev命令

   ```json
   //增加了--history-api-fallback，所有的路由都会指向index.html
   script:{ 
    "dev": "webpack-dev-server --open --history-api-fallback --config webpack.config.js",
   }
   ```


6. 在根实例app.vue里添加一个路由视图`<router-view>`来挂载所有的路由组件。

   ```vue
   <template>
     <div>
         <!-- 添加路由视图来挂载所有的路由组件 -->
         <router-view></router-view>
     </div>
   </template>
      
   <script>
   export default {
     
   }
   </script>
   
   <style scoped>
   </style>
   ```

   - 运行网页时，`<router-view>`会根据当前路由动态渲染不同的页面组件。
   - 网页中一些<u>公共部分</u>，比如顶部的导航栏、侧边导航栏、底部的版权信息，这些也可以写在app.vue里，与`<router-view>`同级。
   - 路由切换时，切换的是`<router-view>`挂载的组件，其他的内容并不会改变。

7. 运行`npm run dev`，分别访问 [127.0.0.1:8080/index](127.0.0.1:8080/index) 和 [127.0.0.1:8080/about](127.0.0.1:8080/about)，就可以看到这两个页面。

   -  两个页面结构如下图所示，两个文件都通过`<script>`标签引入到*index.html*文件的`<header>`中

     - <u>./views/index.vue</u>被打包为一个**0.main.js**文件

       ![127.0.0.1:8080/index](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\router_index.png)

     - ./views/about.vue</u>被打包为一个**1.main.js**文件。

       ![127.0.0.1:8080/about](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\router_about.png)

##### 跳转

###### **vue-router**有两种跳转方式：

1. 使用内置的`<router-link>`组件，它会被渲染为一个`<a>`标签。

​			` <router-link to="/about">跳转到about</router-link>`

- 使用`<router-link>`，在HTML5的History模式下会拦截点击，避免浏览器重新加载页面。

- 其它属性：

  - <u>tag</u>：指定渲染的标签。`<router-link tag="li" to="/about">`渲染的结果就是`<li>`。

  - <u>replace</u>：使用replace不会留下History记录。所以导航后不能用后退键返回上一个页面。

    ​	`<router-link to="/ablout" replace>`

  - <u>active-class</u>：当`<router-link>`对应的路由匹配成功时，会自动给当前元素设置一个名为`router-link-active`的*class*，设置`prop:active-class`可以修改默认的名称。在做类似导航栏时，可以使用该功能高亮显示当前页面对应的导航菜单项，但是一般不会修改*active-class*，直接使用默认值*router-link-active*就可以。

2. 使用`this.$router.push(path)`实现在*JavaScript*中运行跳转页面。

   ```vue
   <template>
       <div>
           <h1>介绍页</h1>
           <button @click="handleRouter">跳转到user</button>
       </div>
   </template>
   <script>
   export default {
       methods:{
           handleRouter(){
               this.$router.push("/user/123");
           }
       }
   }
   </script>
   <style scoped>
   
   </style>
   ```

   - <u>**$router**</u> 还有一些其它的方法

     - <u>replace</u>：类似于`<router-link>`中的`replace`功能，它不会向history添加新纪录，而是替换掉当前的*history*。	

       ​	`this.$router.replace('/user/123');`。	

     - <u>go</u>：类似于`window.history.go()`，在*history*记录中向前或者后退多少步，参数是整数。

       ```js
       //后退一页
       this.$router.go(-1)	;
       //前进两页
       this.$router.go(2);
       ```

##### 高级用法

- 导航钩子`beforeEach`和`afterEach`在**<u>路由即将改变前</u>** 和 **<u>改变后</u>** 触发。

  - 导航钩子有3个参数

    - <u>**to**</u>：即将要进入的目标的路由对象
    - <u>**from**</u>：当前导航即将要离开的路由对象
    - <u>**next**</u>：调用完该方法后，才能进入下一个钩子

  - <u>示例1</u>：实现对页面标题的修改

    - 在路由中自定义信息

      ```js
      const Routers = [
          {
              path: '/index',
              meta: {
                  title: '首页'
              },
              component: (resolve) => require(['./views/index.vue'], resolve)
          },
          //...
      ];
      ```

    - 通过钩子函数`beforeEach`实现标题的设置

      ```js
      //在路由即将改变时，设置标题
      router.beforeEach((to, from, next) => {
          //修改index.html的标题
          window.document.title = to.meta.title;
          next();
      })
      ```

  - <u>示例2</u>：校验是否登录

    ```js
    //如果登录了就可以访问，否则跳转到登录页面
    router.beforeEach((to, from, next) => {
        if (window.localStorage.getItem('token')) {
            next();
        } else {
            next('/login');
        }
    }
    ```

    - <u>**next()**</u>的参数设置为*false*时，可以取消导航，设置为具体的路径可以导航到指定的页面。

  - <u>示例3</u>：解决滚动条位置的问题

    ```js
    //解决前一个页面的滚动条在中间位置，导致跳转页面后，滚动条位置不变的问题router.afterEach((to, from) => {
    router.afterEach((to, from) => {
        window.scrollTo(0, 0);
    })
    ```

### 状态管理与Vuex

#### 概述

- **<u>Vuex</u>** 解决的问题与**中央事件总线（bus）**类似，它作为*Vue*的一个插件来使用，可以更好地管理和维护整个项目的组件状态。

- 在实际业务中，经常有==**跨组件共享数据的需求**==，因此***Vuex***的设计就是用来统一管理组件状态的，它定义了一系列规范来使用和操作数据，使组件应用更加高效。

- 使用*Vuex*有一定的<u>要求</u>：主要使用场景时==**大型单页应用**==，更适合多人协同开发。

#### Vuex基本用法

##### 步骤

1. 使用*npm*安装vuex

   `npm install vuex@2.2.1`

2. 在main.js中配置支持使用*Vuex*

   ```js
   import Vuex from 'vuex';
   
   Vue.use(Vuex);
   
   const store = new Vuex.Store({
       //vuex的配置
   });
   
   new Vue({
       el:'#app',
       router:router,
       //使用vuex
       store:store,
       //...
   })
   ```

仓库*store*包含了应用的数据（状态）和操作过程。Vuex里的数据都是响应式的，任何组件使用同一*store*的数据时，只要*store*的数据变化，对应的组件也会立即更新。

##### 详细配置

###### state

- 据保存在*Vuex* 选项的==<u>state</u>==字段内。

  ```js
  //main.js
  const store = new Vuex.Store({
      //vuex配置
      //数据保存在state字段内
      state: {
          count: 0
      },
  });
  ```

-  在任何组件内，可以直接通过`$store.state.var`读取到变量*count*。

  ```vue
  //index.vue
  <template>
      <div>
          <!--{{$store.state.count}}-->
          <!--使用计算属性实现-->
          {{acount}}
      </div>
  </template>
  <script>
  
  <script>
  export default {
      computed:{
          count:function(){
              return this.$store.state.count;
          }
      },
  }
  </script>
  ```

​		<u>*此时变量只能读取，不能手动改变。*</u>

###### mutations

- ==mutations==用来直接修改**state**里的数据。

  ```js
  const store = new Vuex.Store({
       state: {
          count: 0
      },
      
      //支持直接修改state里的数据，第一个参数固定是state
      mutations: {
          increment(state) {
              state.count++;
          },
          decrease(state) {
              state.count--;
          },
          //支持接受第二个参数，可以是数字、字符串或对象等类型
          //指定每次增加n，默认是2
          increment1(state, n = 2) {
              state.count += n;
          },
          //接受的param是一个对象
          decrease1(state, param) {
              state.count -= param.count;
          }
      }
  })
  ```

- 在任何组件中，可以通过使用$store.commit`直接调用*mutations*里的方法实现修改数据。

  ```vue
  <template>
      <div>
          <h2>{{count}}</h2>
          <button @click="handleIncrease">+1</button>
          <button @click="handleDecrease">-1</button> 
          <button @click="handleIncreaseMore">+5</button>
          <button @click="handleDecreaseMore">-5</button>
      </div>
  </template>
  <script>
  export default {
      computed:{
          count:function(){
              return this.$store.state.count;
          }
      },
      methods:{
          handleIncrease(){
              this.$store.commit("increment");
          },
          handleDecrease(){
              this.$store.commit("decrease");
          },
          //方法一：直接传递第二个参数
          handleIncreaseMore(){
              this.$store.commit("increment1",5);
          },
           //方法二：直接使用包含type属性的对象作为参数
          handleDecreaseMore(){
              this.$store.commit({
                  type:'decrease1', //mumations的方法名
                  count:5     //要传递的参数
              })
          }
      }
  }
  </script>	
  ```

​	【注意】<u>*mutations*里尽量不要异步操作数据。</u> 如果异步操作数据里，组件在`commit`后，数据不能立即改变，而且不知道什么时候会改变。

###### actions

- **<u>actions</u>**和**mutations**h很像，不同的是==*action*里面提交的是*mutation*，并且可以异步操作业务逻辑==。

  ```js
  const store = new Vuex.Store({
       state: {
          count: 0
      },
      
      //支持直接修改state里的数据，第一个参数固定是state
      mutations: {
          increment1(state, n = 2) {
              state.count += n;
          },
      },
  	actions: {
          //使用action来加2
          increment(context) {
              context.commit('increment1');
          },,
          //异步：使用一个Promise在1s后提交mutation，即1s后count更新
          asyncIncrement(context) {
              return new Promise(resolve => {
                  setTimeout(() => {
                      context.commit('increment1');
                  }, 1000)
              });
          }
      },
  }
  ```

- *action*在组件中通过`$store.dispatch`触发

  ```vue
  <template>
      <div>
          <p>测试Vuex中的actions</p>
          <h2>{{aCount}}</h2>
          <button @click="handleActionIncrease">action+2</button>
          <button @click="handleAsyncActionIncrease">async+2</button>
      </div>
  </template>
  <script>
  export default {
      computed:{
          aCount:function(){
              return this.$store.state.count;
          },
      },
      methods:{
          //通过调用actions中的increment()实现count+2
          handleActionIncrease(){
              this.$store.dispatch('increment');
          }，  
          //触发actions中的异步方法asyncIncrement
          //现象：点击按钮后，count会在1s后更新
          handleAsyncActionIncrease(){
              this.$store.dispatch('asyncIncrement').then(()=>{
                  console.log(this.$store.state.count);
              })
          }
      }
  }
  </script>
  ```

  - <u>开发者约定</u>：涉及改变数据的，就使用***mutations***；存在业务逻辑的，就用***actions***。

###### getters

- ==getter==在不改变原数据的基础上，为所有组件提供处理后的新数据

  ```js
  const store = new Vuex.Store({
      //...
  	getters: {
          //返回list数组中所有小于10的数据
          filteredList(state) {
              return state.list.filter(item => item < 10);
          }，
          //第二次参数可以是其它的getters方法，
          //返回上述过滤后的数组中的元素个数
          listCount: (state, getters) => {
              return getters.filteredList.length;
          }
      }
  }
  ```

- 在需要的组件中直接使用getters对应方法

  ```vue
  <template>
    <p>测试Vuex中的getter</p>
    <h2>{{list}}</h2>
    <h2>{{listCount}}</h2>
  </template>
  <script>
  export default {
      computed:{
          //对于Vuex中的list数据，只获取其中小于10的数据
          list(){
              //getters方法，当其它组件也需要过滤后的数据时,直接调用
              return this.$store.getters.filteredList;
          }，
           //返回过滤后的list长度
          listCount:function(){
              return this.$store.getters.listCount;
          }
      },
  }
  </script>
                                                                  
  ```

###### modules

- ==用于将store分隔到不同模块。==当项目足够大时，*store*里的*state*、*mutations*、*actions*、*getters*会非常多，都放在*main.js*里显得不是很友好，使用*modules*可以把它们写进不同的文件中。每个*module*拥有自己的*state*、*mutations*、*actions*、*getters*而且可以多层嵌套。

  ```js
  const moduleA={
      state:{},
      mutations:{},
      actions:{},
      getters:{}
  };
  const moduleB={
      state:{},
      mutations:{},
      actions:{},
      getters:{}
  };
  
  const store = new Vuex.Store({
      modules:{
          a:moduleA,
          b:muduleB
      }
  });
  
  ```

- <u>**传统方式**</u>调用子模块中的状态和方法

  - state

    ```
    //this.$store.state['模块名']['属性名']
    this.$store.state.a.count //moduleA的状态 
    ```

  - actions

    ```
    //this.$store.dispatch('模块名/属性名', opts)
    this.$store.dispatch('a/increment',5)
    ```

  - getters

    ```
    //this.$store.getters.模块对应的属性名
    this.$store.getters.sumCount
    
    //当多个模块的getters存在重名属性时，控制台可以看到一个错误信息：
    //	[vuex] duplicate getter key : xxx
    ```

  - mutations

    ```
    //this.$store.commit('模块对应的属性名')
    this.$store.commit('incrementA')
    
    //当子模块和根模块的mutation同名时，则会先执行根模块，然后按照子模块在modules定义的顺序依次执行
    ```

  ***module*** 的*mutation*和*getter* 接收的第一个参数*state*是当前模块的状态。在*actions*和*getters*中，还可以接收一个参数<u>*rootState*</u>，来访问根节点的状态。

### 实战1：中央事件总线插件vue-bus

1. <u>***vue-bus***</u> 插件像*vue-router*和*Vuex* 一样，给*Vue* 添加一个属性*<u>$bus</u>*，并代理*<u>$emit</u>*、*<u>$on</u>*、*<u>$off</u>* 三个方法。

   ```js
   const install = function (Vue) {
       const Bus = new Vue({
           methods: {
               emit(event, ...args) {
                   this.$emit(event, ...args);
               },
               on(event, callback) {
                   this.$on(event, callback);
               },
               off(event, callback) {
                   this.$off(event, callback);
               }
           }
       });
       Vue.prototype.$bus = Bus;
   };
   export default install;
   ```

​	**【补充】**

- `vm.$on( event, callback )`

  监听当前实例上的自定义事件。事件可以由`vm.$emit`触发。回调函数*callback*会接收所有传入事件触发函数的额外参数。

- `vm.$emit( event, […args] )`

​		触发当前实例上的事件。附加参数都会传给监听器回调，如果没有参数，形式为vm.$emit(event)

- `vm.$off( [event, callback] )`

​		移除自定义事件监听器。如果没有提供参数，则移除所有的事件监听器；

​			1） 如果只提供了事件，则移除该事件所有的监听器；

​			2）如果同时提供了事件与回调，则只移除这个回调的监听器。

2. 在main.js中使用插件

   ```js
   Vue.use(VueBus);
   ```

3. 在组件中使用

   - 子组件*Counter.vue*

     ```vue
     <template>
         <div>
             {{number}}
             <button @click="handleAddRandom">随机增加</button>
         </div>
     </template>
     
     <script>
     export default {
         props:{
             number:{
                 type:Number,
             }
         },
         methods:{
             handleAddRandom(){
                 const num = Math.floor(Math.random()*100+1);
                 this.$bus.emit('add',num);
             }
         }
     }
     </script>
     
     <style scoped></style>
     ```

   - 父组件*index.vue*

     ```vue
     <template>
         <div>
             <h1>验证中央事件总线插件vue-bus</h1>
             <Counter :number="number"></Counter>
         </div>
     </template>
     
     <script>
     import Counter from "./Counter.vue";
     export default {
         components: { Counter },
         data(){
             return {
                 number:0
             }
         },
         created(){
             this.$bus.on('add',num=>{
                 this.number+=num;
             })
         },
     }
     </script>
     
     <style scoped></style>
     ```

4. <u>**注意**</u>

   - ==`$bus.on`应该在created钩子内使用。==如果在<u>*mounted*</u>使用，它可能接收不到其它组件来自<u>*created*</u>钩子内发出的事件。

   - ==因为使用了`$bus.on`，所以在*beforeDestory*钩子里再使用`$bus.off`解除。==。因为组件销毁后，就没必要把监听的句柄储存在*vue-bus*里了。

     ```vue
     //对index.vue进行改进
     <template>
         <div>
             <h1>验证中央事件总线插件vue-bus</h1>
             <Counter :number="number"></Counter>
         </div>
     </template>
     
     <script>
     import Counter from "./Counter.vue";
     export default {
         components: { Counter },
         data(){
             return {
                 number:0
             }
         },
         created(){
             this.$bus.on('add',this.handleAddRandom);
         },
         beforeDestory(){
             this.$bus.off('add',this.handleAddRandom);
         },
         methods:{
             handleAddRandom(num){
                 this.number+=num;
             }
         }
     }
     </script>
     
     <style scoped></style>
     ```

<a id="ajax"></a>

### 实战2：异步获取服务端数据vue-ajax

#### XMLHttpRequest

​	在*JavaScript*中，<u>**XMLHttpRequest**</u> 是客户端的一个 API，它为浏览器与服务器通信提供了一个便捷通道。

##### 概述

1. 创建 XMLHttpRequest 对象

   `var xhr = new XMLHttpRequest();`

2. 建立连接

   在 *JavaScript* 中，使用 *XMLHttpRequest* 对象的 **<u>open()</u>** 方法可以建立一个 HTTP 请求。

   ```js
   // xhr 表示 XMLHttpRequest 对象，open() 方法包含 5 个参数，说明如下：
   // method：HTTP 请求方法，必须参数，值包括 POST、GET 和 HEAD，大小写不敏感。
   // url：请求的 URL 字符串，必须参数，大部分浏览器仅支持同源请求。
   // async：指定请求是否为异步方式，默认为 true。如果为 false，当状态改变时会立即调用 onreadystatechange 属性指定的回调函数。
   // username：可选参数，如果服务器需要验证，该参数指定用户名，如果未指定，当服务器需要验证时，会弹出验证窗口。
   // sword：可选参数，验证信息中的密码部分，如果用户名为空，则该值将被忽略。
   
   xhr.open(method, url, async, username, password);
   ```

3. 发送请求

   建立连接后，可以使用 **<u>send()</u>** 方法发送请求。

   ```js
   xhr.send(body);
   
   //参数 body 表示将通过该请求发送的数据，如果不传递信息，可以设置为 null 或者省略。
   //发送请求后，可以使用 XMLHttpRequest 对象的 responseBody、responseStream、responseText 或 responseXML 属性等待接收响应数据。
   ```

##### Js封装ajax请求

- 说明

```js
/**   
     * js封装ajax请求   
     * >>使用new XMLHttpRequest 创建请求对象,所以不考虑低端IE浏览器(IE6及以下不支持XMLHttpRequest)   
     * >>使用es6语法,如果需要在正式环境使用,则可以用babel转换为es5语法 https://babeljs.cn/docs/setup/#installation   
     * @param settings 请求参数模仿jQuery ajax   
     * 调用该方法,data参数需要和请求头Content-Type对应   
     * Content-Type                        data                                     描述   
     * application/x-www-form-urlencoded   'name=哈哈&age=12'或{name:'哈哈',age:12}  查询字符串,用&分割   
     * application/json                    'name=哈哈&age=12'                        json字符串   
     * multipart/form-data                 new FormData()                           FormData对象,当为FormData类型,不要手动设置Content-Type   
     *  注意:请求参数如果包含日期类型.是否能请求成功需要后台接口配合   
     */
```

- 代码

```js
const http = {
    ajax: (settings = {}) => {
        // 初始化请求参数    
        let _s = Object.assign({
            url: '', // string      
            type: 'GET', // string 'GET' 'POST' 'DELETE'      
            dataType: 'json', // string 期望的返回数据类型:'json' 'text' 'document' ...      
            async: true, //  boolean true:异步请求 false:同步请求 required      
            data: null, // any 请求参数,data需要和请求头Content-Type对应      
            headers: {}, // object 请求头      
            timeout: 1000, // string 超时时间:0表示不设置超时      
            beforeSend: (xhr) => {},
            success: (result, status, xhr) => {},
            error: (xhr, status, error) => {},
            complete: (xhr, status) => {}
        }, settings);
        // 参数验证    
        if (!_s.url || !_s.type || !_s.dataType || !_s.async) { alert('参数有误'); return; }
        // 创建XMLHttpRequest请求对象    
        let xhr = new XMLHttpRequest();
        // 请求开始回调函数    
        xhr.addEventListener('loadstart', e => { _s.beforeSend(xhr); });
        // 请求成功回调函数    
        xhr.addEventListener('load', e => {
            const status = xhr.status;
            if ((status >= 200 && status < 300) || status === 304) {
                let result;
                if (xhr.responseType === 'text') {
                    result = xhr.responseText;
                } else if (xhr.responseType === 'document') {
                    result = xhr.responseXML;
                } else {
                    result = xhr.response;
                }
                // 注意:状态码200表示请求发送/接受成功,不表示业务处理成功        
                _s.success(result, status, xhr);
            } else {
                _s.error(xhr, status, e);
            }
        });
        // 请求结束    
        xhr.addEventListener('loadend', e => {
            _s.complete(xhr, xhr.status);
        });
        // 请求出错    
        xhr.addEventListener('error', e => {
            _s.error(xhr, xhr.status, e);
        });
        // 请求超时    
        xhr.addEventListener('timeout', e => {
            _s.error(xhr, 408, e);
        });

        let useUrlParam = false;
        let sType = _s.type.toUpperCase();
        // 如果是"简单"请求,则把data参数组装在url上    
        if (sType === 'GET' || sType === 'DELETE') {
            useUrlParam = true;
            _s.url += http.getUrlParam(_s.url, _s.data);
        }
        // 初始化请求    
        xhr.open(_s.type, _s.url, _s.async);
        // 设置期望的返回数据类型    
        xhr.responseType = _s.dataType;
        // 设置请求头    
        for (const key of Object.keys(_s.headers)) {
            xhr.setRequestHeader(key, _s.headers[key]);
        }
        // 设置超时时间    
        if (_s.async && _s.timeout) {
            xhr.timeout = _s.timeout;
        }
        // 发送请求.如果是简单请求,请求参数应为null.否则,请求参数类型需要和请求头Content-Type对应    
        xhr.send(useUrlParam ? null : http.getQueryData(_s.data));
    },
    // 把参数data转为url查询参数  
    getUrlParam: (url, data) => {
        if (!data) {
            return '';
        }
        let paramsStr = data instanceof Object ? http.getQueryString(data) : data;
        return (url.indexOf('?') !== -1) ? paramsStr : '?' + paramsStr;
    },
    // 获取ajax请求参数  
    getQueryData: (data) => {
        if (!data) {
            return null;
        }
        if (typeof data === 'string') {
            return data;
        }
        if (data instanceof FormData) {
            return data;
        }
        return http.getQueryString(data);
    },
    // 把对象转为查询字符串  
    getQueryString: (data) => {
        let paramsArr = [];
        if (data instanceof Object) {
            Object.keys(data).forEach(key => {
                let val = data[key];
                // todo 参数Date类型需要根据后台api酌情处理        
                if (val instanceof Date) {
                    // val = dateFormat(val, 'yyyy-MM-dd hh:mm:ss');       
                }
                paramsArr.push(encodeURIComponent(key) + '=' + encodeURIComponent(val));
            });
        }
        return paramsArr.join('&');
    }
}
```



# [Webpack](https://www.webpackjs.com/)

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



# 实战：知乎日报项目开发

## 跨域限制

- 跨域限制是服务端的一个行为，当开启对某些域名的访问限制后，只有同域或指定域下的页面可以调用，这样相对来说更安全，图片也可以防盗链。

- 跨域限制一般只在浏览器端存在，对于服务端或iOS、Android等客户端是不存在的。

- 使用**<u>代理</u>** 是开发过程中常见的一种解决方案：

  - 使用*npm*安装 request

    `npm install --save-dev request`

  - 创建**<u>proxy.js</u>**文件

    ```js
    const http = require('http');
    const request = require('request');
    
    const hostname = '127.0.0.1';
    const port = 8010;
    const imgPort = 8011;
    
    
    //创建一个API代理服务
    const apiServer = http.createServer((req, res) => {
        //知乎日报的接口地址的前缀：http://news-at.zhihu.com/api/4
        const url = 'http://news-at.zhihu.com/api/4' + req.url;
        const options = {
            url: url
        };
        function callback(error, response, body) {
            if (!error && response.statusCode === 200) {
                //设置编码类型，否则中文会显示为乱码
                res.setHeader('Content-Type', 'text/plain;charset=UTF-8');
                //设置所有域允许跨域
                res.setHeader("Access-Control-Allow-Origin", "*");
                //返回代理后的内容
                res.end(body);
            }
        }
        req.get(options, callback);
    });
    
    //监听8010端口
    apiServer.listen(port, hostname, () => {
        console.log(`接口代理运行在http://${hostname}:${port}/`);
    });
    
    //创建一个图片代理服务
    const imgServer = http.createServer((req, res) => {
        const url = req.url.split('/img/')[1];
        const options = {
            url: url,
            encoding: null
        }
        function callback(error, response, body) {
            const contentType = reponse.headers['contetn-type'];
            res.setHeader('Content-Type', contentType);
            res.setHeader('Access-Control-Allow-Origin', "*");
            res.end(body);
        }
        req.get(options, callback);
    });
    //监听8011端口
    imgServer.listen(imgPort, hostname, () => {
        console.log(`图片代理运行在http://${hostname}:${imgPort}/`);
    })
    ```

    - 真实请求接口为 http://news-at.zhihu.com/api/4/news/3892357，开发时改写为 http://127.0.0.1:8010/news/3892357。
    - 图片的真实地址为 https://pic4.zhimg.com/v2-b44636ccd2affac97ccc0759a0f46f7f.jpg，开发时改写为 http://127.0.0.1:8011/img/https://pic4.zhimg.com/v2-b44636ccd2affac97ccc0759a0f46f7f.jpg。

  - 在终端使用*Node* 启动代理服务

    `node ./proxy.js`

    ![node执行代理](C:\Users\10195\Desktop\笔记\Notes\images\Vue.js\node执行代理.png)









[^1]:因为普通对象没有Symbol.iterator属性，如果一个对象拥有Symbol.iterator属性，那么就可以使用for...of遍历。
[^2]:当浏览器加载页面后看到的第一眼的显示内容为首屏。
