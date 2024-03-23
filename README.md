
#### Official React API

target

![](https://i.imgur.com/K4plJAM.png)


今天的目标是在页面中呈现“APP” 这三个字符
当然，呈现是要有要求的。
我们写的这个mini react， 它的api是要和官方的React对齐的
我们先看看React官方的API是什么样子的。

Today, we’re aiming to get the letters ‘APP’ on the page. 
But there’s a catch: our mini-react has to match up with the official React API. 
Let’s take a look at the official React API.”

以下是我已经创建好的react项目

Right, this here is the React project 
I’ve gone ahead and sorted out beforehand.
![](https://i.imgur.com/T0sY9aw.png)


里面有两个jsx， app.jsx是我们的根主件
Inside, we've got two JSX files, and app.jsx is our root component

![](https://i.imgur.com/uZprupM.png)

对应的是以下
The corresponding bit is as follows
![](https://i.imgur.com/jy93WM4.png)

main.jsx,实际上是作为一个主入口，在这个文件里面，它主要调用了ReactDOM.createRoot（）,
然后传入了根容器，调用成功之后，它会返回一个对象，然后在这个对象里会去调用这个render函数，把我们的根主见传过去，然后会在内部里面创建好一个对应的视图

main.jsx essentially acts as the main entry point. 
In this file, it mainly uses ReactDOM.createRoot(), passing in the root container. 
After that’s successful, it returns an object which then calls the render function, sending our root component through to eventually create the corresponding view inside.

![](https://i.imgur.com/5zE8fxJ.png)


这个根容器是在index.html里面，已经生成好的。
sThe root container’s already set up in the index.html.
![](https://i.imgur.com/SDRaxiB.png)


#### Mini-react

知道目标之后，我们就可以开始进行。那么，我们在进行的时候，我们是采用一种渐进式的方式。我们先从最简单的方式入手，然后一点点去过渡到更复杂，更通用的一种方式。
那我们就开始吧。

Now we know what we’re aiming for, we can crack on. 
We’ll take it step by step, starting simple and gradually moving to more complex and versatile methods. 
So, let’s get started.

`touch index.html`

`!html`

in the body, we can add `<body> <div id="root"> </div>`


然后我们再去引入一个脚本 
we can add another script tag `<srcipt type="module" src="main.js"></script>`
Then we are bringing in another script

so we can create a file `main.js`

首先，第一步的时候呢，我们要小步走。先看看main.js有没有成功地被引入
`console.log("manjs");`

First off, we’re taking baby steps. 

Let’s see if main.js has been successfully brought in.”

用浏览器打开index.html，然后右键点击浏览器，然后点击Inspect
Open index.html in your browser, right-click, and then click 'Inspect'.

然后，我们可以进行下一步。
Then, we can move on to the next step.

那我们就要去思考，怎么让它显示一个 “app” 呢？
So, we need to think about how to display the letters 'APP' on the browser.

最简单的方式是用dom 去实现
The simplest way is to use the DOM (Document Object Model (DOM).

我们创建一个dom， 等于 document.createElement(""), 然后给一个点吧（“div”）;

We create a DOM element like so: `document.createElement("")`, and let's give it a value with a 'div'.

this line of code is creating a new, empty `<div>` element and storing a reference to it in the `dom` variable. This element is not yet visible on the page because it hasn't been added to the document. To make it visible, you would need to append this newly created `<div>` to an existing element in the DOM. For example, you could use `document.body.appendChild(dom);` to add the `<div>` to the body of the document, making it visible on the page.

[[Web Programming >> 001]]

```// v1
const dom = document.createElement("div");

```


接下来我们可以设置下它的id

```
dom.id = "app"
```

再接下来，我们可以把我们的根容器，去append在dom

```
document.querySelector('#root').append(dom);
```

只是这样的话，我们只有div
我们还需要去设置它内部的字符内容app

```
const textNode = document.createTextNode("");
textNode.nodeValue = "app";
```

再接下来，我们把这部分添加到dom里面

```
dom.append(textNode);
```


但是，我们现在是把这个东西写死的。

没关系，我们进行到下一个版本。

我们都知道React是基于这个虚拟dom - virtual dom

那么这个虚拟dom是什么呢？是不是就是一个普通的js object
它通过对象的形式描述出它的dom的一个形状
我们也可以引入这个对象，去描述出这个dom的形状是什么样子的

首先，我们要分析一下，我们需要什么。

第一个，我们需要这个类型 div，我们创建的是一个element
这个element是什么标签呢
这个标签是通过类型来进行区分
![](https://i.imgur.com/pfTjbpe.png)

第二个是我们的props，属性

![](https://i.imgur.com/hdqbyo8.png)

我们dom的结构是什么，是一棵树
node - 子节点
![](https://i.imgur.com/Gf3MtPD.png)

所以总结下来，我们需要三个
type props children

接下来，我们就给它创建呗。

```
const el = {
			type:"div",
			props: {
					id: "app",
					children: [
						{
							type: "TEXT_ELEMENT"
							props: {
								nodeValue: "app",
								children: []
							}
						}
					]
			}
}
```

类型是一个diy
props是一个id
然后我们还有一个children
children里面，其实又是一个对象

当然，我们这个textNode是一个比较特殊， 所以给它一个“TEXT_ELEMENT"
那么，在这个children也会有一个props

我们也给这个children，再来一个children，因为方便我们知道每一个children都会有一个children
就不需要去检测有没有children，这样的话，我们计算起来就方便一些。

这里有一个要点，我们去写程序的时候，如果你感觉你的算法比较复杂，可以回头思考一下你的数据结构是不是足够的简单，因为只要的你的数据结构设计得比较好的话，可以减少算法的复杂度


下一步，我们可以进行简单地优化一下

```

const textEl = {
	type: "TEXT_ELEMENT",
	props: {
		nodeValue: "app",
		children: [],
	},
}；

const el = {
	type: "div",
	props:{
		id = "app",
		children = [textEl],
	},
}；
```


首先，我们可以把`"div"` 更换成

```
const dom = document.createElement(el.type);
const dom.id = el.props.id;


document.querySelector("#root").append(dom);

const textNode = document.createTextNode("");
textNode.nodeValue = textEl.props.nodeValue;
dom.append(textNode);
```


在第二版本里面，我们就有了virtual dom这个概念了。但依然是写死的

那么，我们是否可以让它动态进行创建呢？


对于我们的textEl来讲呢，我们可以写个函数 `function createTextNode(){}`

```
function createTextNode(text){
}
```

我们直接把这个拷贝过来

```
type: "TEXT_ELEMENT",
	props: {
		nodeValue: "app",
		children: [],
	},
```


```
function createTextNode(text){

	return {
		type: "TEXT_ELEMENT",
	props: {
		nodeValue: text,
		children: [],
	},
	}
}

```


我们把这个注释掉

```
const textEl = {
	type: "TEXT_ELEMENT",
	props: {
		nodeValue: "app",
		children: [],
	},
};
```


然后我们可以写一个函数去动态创建

```
function createElement(){

}
```

这里的变化点可就多了，

```
function createElement(type, props, ...children){
	return {
		type,
		props: {
			...props,
			children,
		},
	};
}
```


我们只需要加入`...` , 就变成数组了


然后我们就可以这样写了

```
const textEl = createTextNode("app");
const App = createElement("div", {id:"app"}, textEl);
```


```
const dom = document.createElement(App.type);
dom.id = App.props.id;
document.querySelector("#root").append(dom);

const textNode = document.createTextNode("");
textNode.nodeValue = textEl.props.nodeValue;
dom.append(textNode);
```


接下来，我们要去思考如何动态去创建我们的dom节点



![](https://i.imgur.com/JIXjM1o.png)



我们要分析一下，这两个部分有没有共同点?

首先，第一步呢，是不是都是去创建一个节点，但是创建节点的时候有点区别

一个是`createElement`, 另外一个是`createTextNode`, 他们的一个抽象动作，都是去
然后，第二步是非常明显，去设置props

最后呢，第三个都是让它的父级去添加 （append）


因此，我们就基于这三个步骤，去给它实现一下。

v3
我们去创建一个render 函数
```
function render(){
}
```

我们第一步是去创建 dom

```
function redner(el){
	const dom = el.type == "TEXT_ELEMENT" ? document.createTextNode("") : document.createElement(el.type); 
}
```

这个`el` 哪里来了，从函数里面传过来

第二步骤呢，我们去设置它的props, 我们设置props呢，会有多个
设置id，设置class
我们这边是给它便利？？？
```
function redner(el){
	const dom = el.type == "TEXT_ELEMENT" ? document.createTextNode("") : document.createElement(el.type); 

	Object.keys(el.props).forEach((key) => {
		if (key != "children"){
			dom[key] = el.props[key];
		}
	})
}
```

我们拿到key之后，还是要去做有个检测，有的时候我们不能直接处理我们的children，children 的话应该单独处理


第三个，我们是给它添加一个容器, 这个容器是从函数传过来
```
function redner(el, container){
	const dom = el.type == "TEXT_ELEMENT" ? document.createTextNode("") : document.createElement(el.type); 

	Object.keys(el.props).forEach((key) => {
		if (key != "children"){
			dom[key] = el.props[key];
		}
	})
	container.append(dom)
}
```


第四步的话，别忘了处理children

```
function redner(el, container){
	const dom = el.type == "TEXT_ELEMENT" ? document.createTextNode("") : document.createElement(el.type); 

	Object.keys(el.props).forEach((key) => {
		if (key != "children"){
			dom[key] = el.props[key];
		}
	})
	const children =  el.props.children
	children.forEach(child => {
		render(child, dom)
	});
	container.append(dom)
}
```

我们拿到这个孩子，孩子是在`el.props.children`属性上
然后，我们需要便例一下？

因为我们创建dom的每个动作都是一样的
所以我们在这边再次调用一下函数，其实就是一种递归
==容器是什么，就是我们创建这个dom== (it's actually weird, why is dom represents container)


```

function redner(el, container){
	const dom = el.type == "TEXT_ELEMENT" ? document.createTextNode("") : document.createElement(el.type); 

	Object.keys(el.props).forEach((key) => {
		if (key != "children"){
			dom[key] = el.props[key];
		}
	})
	const children =  el.props.children
	children.forEach(child => {
		render(child, dom)
	});
	container.append(dom)
}

const textEl = createTextNode("app");
const App = createElement("div", {id: "app", textEl});
render(App, document.querySelector("#root"))

```


继续优化，我们想直接传`app`,而不是传 `textEl`, 直接传"app"， 显然是不行的。

我们只需要在函数 createElement(type, props, ...children) 做一个改变

```
function createElement(type, props, ...children){
	return {
		type,
		props: {
			...props,
			children: children.map(child) => {
				return typeof child == "string"? createTextNode(child) : child;
			}
		}
	}
}
```

我们给它map一下，看下是不是等于它一个string类型，如果事的话，我们就调用这个函数 `createTextNode(child)`, 不是的话，就把这个child 返回出去

我们这边可以加几个

我们可以在开发者工具看看这个结构，是否与这个之前写死的一样

![](https://i.imgur.com/VhrcRY5.png)

里面有个children， 然后又有两个 textelement
![](https://i.imgur.com/JNvkFwa.png)



我们的目标是什么？ 

我们需要构建对应的

我们需要一个ReactDom，首先我们创建一个ReactDom, 其实又是一个对象。

```
const ReactDom = {

}
```

里面有一个`createRoot(){}`

```
const ReactDom = {
	createRoot(){
		
	}
}
```

当我们调用createRoot，我们需要一个容器，以及`createRoot`需要返回一个对象,在这个对象上，是有一个render函数

```
cosnt ReactDom ={
	createRoot(container) {
		return {
			render(){
				
			}
		}
	}
}
```

在这个render函数里面，传入的是什么，是根容器

```
cosnt ReactDom ={
	createRoot(container) {
		return {
			render(App){
				render(App, container)
				
			}
		}
	}
}
```

==I dont understand why there is only one parameter app in render(App) but two parameters { render(App, container)}==


最后呢，我们需要重构一下我们代码。

把我们业务代码，和mini-react（框架代码）给拆分开

比如说我们这边，创建一个文件夹/文件-> `core/React.js`, `core/ReactDom.js`

因为
```
import ReactDOM from "react-dom/client";
import App from "./App.jsx"
```
所以，
in ReactDom.js

```
const ReactDOM = {
	createRoot(container) {
		return {
			render(App){
				render(App, container);
			}
		}
	}

}

export default ReactDOM
```

然后，我们再给它默认导出


in React.js

```
function createTextNode(text){
	return {
		type: "TEXT_ELEMENT",
		props: {
			nodeValue: text,
			children: [],
		},
	};
}

function createElement(type, props, ...children){
	return {
		type, 
		props: {
			...props,
			children: children.map((child) => {
				return typedef child == "string" ? createTextNode(child) : child;
			}),
		},
	}
}

function render(el, container){
	const dom = el.type == "TEXT_ELEMENT" ? document.createTextNode("") : doucment.createElement(el.type);

	Object.keys(el.props).forEach((key) => {
		if (key != "children") {
			dom[key] = el.props[key];
		}
	});

	const children = el.props.children;
	children.forEach( (child) => {
		render(child, dom);
	});

	container.append(dom);

}

const React = {
	render,
	createElement,
};

export default React
```



这个时候，我们已经构建好了，我们还需要去处理下 ReactDOM，
因为在ReactDom.js这边，是需要调动React.js里面的render函数


所以说，我们需要在


```

import React from './React.js';

const ReactDOM = {
	createRoot(container) {
		return {
			render(App){
				React.render(App, container);
			}
		}
	}

}

export default ReactDOM
```


in the `main.js`


```
import ReactDOm from './core/ReactDom.js'
import React from './core/React.js'

const App = React.createElement("div", {id: "app"}, "hi-", "mini-react");

ReactDOM.createRoot(document.querySelector('#root')).render(App)
```


再进一步，就是构建`App.js`

```
import React from './core/React.js'

const App = React.createElement("div", {id: "app"}, "hi-", "mini-react");

export deafult App
```


```
import ReactDOm from './core/ReactDom.js';
import App from './App.js';

ReactDOM.createRoot(document.querySelector('#root')).render(App)
```
