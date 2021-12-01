# Vue.js入门

## 一个.vue文件

### 组成部分

#### HTML：页面框架。

​	<i>使用 `<template></template>`标识。</i>

#### JS：交互。

​	<i>使用 `<script></script>`标识。</i>

#### CSS：样式。

​	<i>使用 `<style></style>`标识。</i>

```vue
<template>
	<div>
        ...
    </div>
</template>

<script>
    ...
</script>

<style>
    ...
</style>
```

### 双向绑定

#### HTML和JS互相影响

- HTML中的<b>{{message}}</b> 影响js中的`data.message`

  ```html
  <div id="demo">
      <p>{{message}}</p>
      <input v-model="message"/>
  </div>
  ```

  ```javascript
  let vue = new Vue({
      el:'#demo',
      data:{
          message:'hello vue!'
      }
  })
  ```

## 第一个Vue项目

### 创建和启动

#### 使用 <b>vue-cli</b> 脚手架创建新项目

​	`vue init webpack my-first-vue-project`

#### 安装依赖包

​	`npm install`

#### 启动

​	`npm run dev`

### Vue.js的重要选项

#### data：vue对象的数据

#### methods：vue对象的方法

#### watch：对象监听的方法

```js
new Vue({
data:{
	a:1,
	b:[]
},
methods:{
	doSomething:function(){
		this.a++;
	}
},
watch:{
	'a':function(val,oldVal){
		console.log(val,oldVal); //输出：2,1
	}
}
})
```

### 模板指令 

​	html和vue对象的粘合剂

#### 数据渲染

- <b>v-text</b>：处理html

- <b>v-html</b>：保存html结构

- <b>{{}}</b>

  ```vue
  new Vue({
  	data:{
  		a:1,
  		b:[]
  	}
  })
  
  <p>{{a}}</p>
  <p v-text="a"></p>
  <p v-html="a"></p>
  ```

#### 控制模块隐藏

- <b>v-if</b>：直接不渲染dom元素
- <b>v-show</b>：通过css的属性对其隐藏

    ```vue
    new Vue({
        data:{
            isShow:true
        }
    })
    
    <p v-if="isShow"></p>
    <p v-show="isShow"></p>

#### 循环渲染列表

- <b>v-for</b>

    ```vue
    new Vue({
        data:{
            items:[
    			{label:'apple'},
    			{label:'banana'}
    		]
        }
    })
    
    <ul>
        <li v-if='item in items'>
        	<p v-text='item.label'></p>
        </li>
    </ul>
    ```

#### 事件绑定

- v-on

  `v-on:click`可以简写成`@click`

	```vue
	new Vue({
   ...
	methods:{
		doThis:function(something){}
	}
	})
	
	<button v-on:click="doThis"></button>
	<button @click="doThis"></button> 
	```

#### 属性绑定

- v-bind

  `v-bind:class`可以简写成`:class`

  ```vue
  <img v-bind:src="imageSrc">
  
  <div :class="{red:isRed}"></div>
  <div :class="[classA,classB]"></div>
  <div :class="[classA,{classB:isB,classC:isC}]"></div>
  ```



