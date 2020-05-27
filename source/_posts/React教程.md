---
title: React教程
date: 2020-05-27 17:01:18
tags:
---

React是一个用于构建用户界面的javascript库  
React实例：  
```javascript  
<div id="example"></div>
<script type="text/babel">
  ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
  );
</script>
```
  
---

> <font size="2">Node.js是一个基于Chrome V8引擎的JavaScript运行环境。简单的说，Node.js是运行在服务端的JavaScript，Node.js使用了一个事件驱动、非阻塞式I/O的模型。  
npm是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题。  
国内使用 npm 速度很慢，可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm。  

<h4>通过npm使用React</h4>  

```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
$ npm config set registry https://registry.npm.taobao.org
$ cnpm install -g create-react-app
$ create-react-app my-app
$ cd my-app/
$ npm start
```

---

<h4>React元素渲染</h4>  
元素是构成React应用的最小单位，它用于描述屏幕上输出的内容。  

`const element = <h1>Hello, world!</h1>;`

与浏览器的DOM元素不同，React当中的元素事实上是普通的对象，React DOM可以确保浏览器DOM的数据内容与React元素保持一致。
<h4>将元素渲染到DOM中</h4>  
首先我们在一个HTML页面中添加一个id="example"的div：  

`<div id="example"></div>`

在此div中的所有内容都将由React DOM来管理，所以我们将其称为根DOM节点。  
要将React元素渲染到根节点中，我们通过把它们都传递给ReactDOM.render()的方法来将其渲染到页面上： 

 
```
const element = <h1>Hello, world!</h1>;
ReactDOM.render(
    element,
    document.getElementById('example')
);
```