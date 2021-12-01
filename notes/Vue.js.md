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

# CSS3

## 1.滑动动画

​	![滑动动画](C:\Users\10195\Desktop\笔记\images\滑动动画-16377529442701.gif)

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

# 自定义指令

## 1.变量声明

- 使用`el.__xxx__`在上下文中声明一个变量，不能使用`this.xxx`
- 在`bind()`方法中添加监听方法，还需要在`unbind()`方法中移除该监听，使用中间变量`el.__xxx__`

# Virtual Dom

`Virtual Dom`并不是真正意义上的DOM，而是一个轻量级的**JavaScript对象**。

## 运行过程

![Virtual Dom运行过程](C:\Users\10195\Desktop\笔记\images\Virtual Dom运行过程.PNG)

## VNode

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

## createElement

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

