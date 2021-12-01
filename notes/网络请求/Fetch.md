## 与Ajax对比

- 业务逻辑更清晰
- 代码更简洁
- 可读性更好

```js
// 使用Ajax请求一个JSON数据
var xhr = new XMLHttpRequest();
xhr.open('GET',url/file,true);
xhr.onreadystatechange=function(){
	if(xhr.readyState==4){
		if(xhr.status==200){
			var data = xhr.responseText;
			console.log(data);
		}
	}
};
xhr.onerror = function(){
    console.log("Oh,error");
};
xhr.send();

//使用fetech请求JSON数据
fetch(url).then(response=>response.json()) //解析为刻度数据
		.then(data=>console.log(data)) //执行结果是resolve就调用then方法
		.catch(err=>console.log("Oh,error",err)) //执行结果是reject就调用catch方法
```

## 优点

- 语法简介，更加语义化

- 基于标准Promise实现，支持async/await

- 同构方便，使用isomorphic-fetch

## promise简介

![promise简介](C:\Users\10195\Desktop\笔记\images\promise简介-网络请求-fetch.PNG)

- 一切正常（resolved）
  - 运行then方法
- 出问题了（rejected）
  - 运行catch方法

## 请求常见数据格式

```html
<div class="container">
    <h1>Fetch Api sandbox</h1>
    <button id="button1">请求本地文本数据</button>
    <button id="button2">请求本地JSON数据</button>
    <button id="button3">请求网络接口</button>
    <br><br>
    <div id="output"></div>
</div>
<script src="app.js"></script>
```

### 请求本地txt文档

本地有一个test.txt文档，通过以下代码就可以获取其中的数据，并且显示在页面上。

```js
document.getElementById('button1').addEventListener('click',getText);
function getText(){
    fetch("test.txt")
    	.then((res)=>res.text()) //注意：此处是res.text()
    	.then(data=>{
            console.log(data);
            document.getElementById('output').innerHTML=data;
    })
    .catch(err=>console.log(err));
}
```

###  请求本地JSON数据

本地有个posts.json数据，与请求本地文本不同的是，得到数据后还要==用forEach遍历==，最后呈现在页面上。

```js
document.getElementById('button2').addEventListener('click',getJson);
function getJson(){
    .then((res)=>res.json())
    .then(data=>{
        console.log(data);
        let output='';
        data.forEach((post)=>{
            output+=`<li>${post.title}</li>`;
        })
        document.getElementById('output').innerHTML=output;
    })
    .catch(err=>console.log(err));
}
```

### 请求网络接口

获取https://api.github.com/users中的数据，做法与获取本地JSON的方法类似，得到数据后，同样要经过处理。

```js
document.getElementById('button3').addEventListener('click',getExternal);
function getExternal(){
    fetch("https://api.github.com/users")
    	.then((res)=>res.json())
    	.then(data=>{
        	console.log(data);
        	let output='';
        	data.forEach((user)=>{
            	output+=`<li>${user.login}</li>`;
        	})
			document.getElementById('output').innerHTML=output;
    	})
    	.catch(err=>console.log(err));
}
```



