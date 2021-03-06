

## **ajax方法**使用（XMLHttpRequest） 

### AJAX 历史

​		AJAX（Asynchronous JavaScript and XML），最早出现在 2005 年的 Google Suggest，是在浏览器端进行网络编 程（发送请求、接收响应）的技术方案，它使我们可以通过 JavaScript 直接获取服务端最新的内容而不必重新加载 页面。让 Web 更能接近桌面应用的用户体验。

​		**AJAX 就是浏览器提供的一套 API，可以通过 JavaScript 调用，从而实现通过代码控制请求与响应。实现 网络编程**。



### 同步异步

**在计算机中同步与异步正好相反**



**js中的同步异步**

- 同步：每一件事件按顺序执行，做完一件事在去做另一件事件
- 异步：同时进行多项任务的操作，当异步任务完成之后，主线程获取执行结果



> ***一定在发送请求 send() 之前注册 readystatechange （不管同步或者异步）***



### XMLHttpRequest对象主要API

**所有方法调用之前，首先进行实例化对象的操作 ** var xhr = new XMLHttpRequest()

| 方法名             | 使用方法                                                     | 类型     |
| ------------------ | ------------------------------------------------------------ | -------- |
| open               | 创建一个新的http请求，并指定此请求的方法、URL以及验证信息<br />open(requestMethod,requestUrl) | function |
| send               | 创建http请求对象完成，开始发送请求<br />body 请求的主体，如果没有请求主体，body位置处为null(get方法)<br />send(当为post请求时，传递本次请求参数,get请求使用null为参数) | function |
| onreadystatechange | 监听本次基于响应的事件                                       | function |
| setRequestHeader   | 设置本次请求请求头信息  <br /> xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded') | function |
| responseText       | 本次请求响应体                                               | string   |
| readyState         | 本次请求相依状态   0 - 4 之间的数字<br /><br />**0 ： XHR方法已经创建，但尚未调用open方法<br />1 ：open() 方法已经被调用，建立了连接。<br />2： send() 方法已经被调用，并且已经可以获取状态行和响应头<br />3：响应体下载中， responseText 属性可能已经包含部分数据。<br />4：响应体下载完成，可以直接使用 responseTex** | number   |
| status             | 本次请求后端返回响应结果状态码  200 32 303  404 403 等       | number   |



### GET 请求

```js
var xhr = new XMLHttpRequest()

// GET 请求传递参数通常使用的是问号传参
// 这里可以在请求地址后面加上参数，从而传递数据到服务端
xhr.open('GET', 'http://localhost:3001?page=1&size=20')

xhr.onreadystatechange = function () {
	if (this.readyState === 4) {
		console.log(this.responseText)
	}
}

// 一般在 GET 请求时无需设置请求，可以传 null 或者干脆不传
xhr.send(null)

// 一般情况下 URL 传递的都是参数性质的数据，而 POST 一般都是业务数据
```



### POST请求

```jsx
var xhr = new XMLHttpRequest()

// open 方法的第一个参数的作用就是设置请求的 method
xhr.open('POST', 'http://localhost:3001/getUserInfo')

// 设置请求头中的 Content‐Type 为 application/x‐www‐form‐urlencoded
// 标识此次请求的请求体格式为 urlencoded 以便于服务端接收数据
xhr.setRequestHeader('Content‐Type', 'application/x‐www‐form‐urlencoded')

xhr.onreadystatechange = function () {
	if (this.readyState === 4) {
		console.log(this.responseText)
	}
}

// 需要提交到服务端的数据可以通过 send 方法的参数传递
// 格式：key1=value1&key2=value2
xhr.send('key1=value1&key2=value2')

```



### 常见返回值格式

- **JSON** 比较通用的请求数据方式

  ```
  {"citys":[{"city":"大庆"},{"city":"哈尔滨"}]}
  ```

  

- **XML**   传统的请求数据的方式     

  ```csharp
  <citys>
        <city>哈尔滨</city>
        <city>大庆</city>
  </citys>  
  ```

  

### 封装方法实现

**调用方法参数为Object类型**

| name      | type                        | 必选 | 说明                                                         |
| --------- | --------------------------- | ---- | ------------------------------------------------------------ |
| url       | string                      | 是   | 本次请求的请求地址                                           |
| method    | string  =>  "get \|\| post" | 否   | 本次请求的请求方法  支持GET、POST请求，不进行传递，默认为get请求 |
| data      | object => {anyKey:valye}    | 否   | 请求携带的参数，当请求没有参数，可以不进行传递               |
| type      | string  =>  "json"          | 否   | 响应值类型，传递值为json 时，返回json对象，没有传递，返回JSON字符串 |
| onSuccess | fucntion                    | 是   | 请求成功的回调函数，默认形参为前台调用成功的响应值           |
| onError   | function                    | 否   | 请求失败的回调函数，默认形参为本次请求失败的描述信息         |



#### 实现流程

1. 处理默认参数，当三方进行调用的时候，如果没有进行参数传递，ajax方法内部定义默认参数
2. 定义ajax基本结构（实例化、open、onreadystatechange、send）
3. 处理请求参数GET请求及POST请求（参数拼接）
4. 监听响应状态（失败成功）



## 瀑布流实现



### 需求分析

1. 初始化，界面渲染首屏数据（图片）
2. 滚动条滚动，请求数据，加载第二屏数据
3. 继续滚动，实现第二步操作



### 知识点

#### 节流的使用

#### 文件异步响应处理方法

#### 图片懒加载



### 接口请求地址

该接口为cors接口，前端直接访问即可，请求方法 GET请求，请求参数：无

https://api.hyfarsight.com/test/testRequest/imgList













