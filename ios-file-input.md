#### 原因：当js创建的input未append进文档流时，Android和pc正常触发onChange事件，ios不触发onChange事件；
#### 解决方法：将js创建的input未append进文档流，style设置为不可见即可
````js
const input = document.createElement('input')
input.style.display = 'none'
input.type = 'file'
let accept = 'video/mp4'
document.body.appendChild(input)
input.click()
input.addEventListener('change', (e) => {
console.log(e)
...
document.body.removeChild(input)
})
````

作者：羊驼626
链接：https://www.jianshu.com/p/d621e71ce265
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


为什么vConsole输出之后可以
输出??是没效果的，只有输出this或者this.el可用，因为输出后对应将他渲染上去了，不然无法显示

针对DocumentFragment的理解

> 作者：cuteJack_杭州               
> 链接：https://juejin.cn/post/6952499015879507982             
> 来源：稀土掘金                  
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

---------

## 什么是文档片段？

（MDN解释：）DocumentFragment，文档片段接口，一个没有父对象的最小文档对象。它被作为一个轻量版的 Document 使用，就像标准的document一样，存储由节点（nodes）组成的文档结构。

### 作用是什么

与document相比，最大的区别是DocumentFragment 不是真实 DOM 树的一部分，它的变化不会触发 DOM 树的重新渲染，且不会导致性能等问题。

### 怎么使用DocumentFragment

```js
    let fragement = document.createDocumentFragment()
    console.log(fragement.nodeName)//#document-fragment
    console.log(fragement.nodeType)//11
    console.log(fragement.nodeValue)//null
    console.log(fragement.parentNode)//null
```

通常情况下，我们使用Node.appendChild 和 Node.insertBefore 的方法，往dom元素添加元素。每个节点分别被插入到文档中，这样会发生多次重新渲染，造成页面回流的操作，则会非常消耗性能，影响用户体验。

### DocumentFragment解决了什么？

使用DocumentFragment能解决直接操作DOM引发大量回流的问题，因为所有的节点会被一次插入到文档中，而这个操作仅发生一个重渲染的操作，比如我们要给ul添加五个li节点，区别就像这样： 直接操作DOM，回流五次：



```js
                let app = document.querySelector('.app')
		for(let i = 0;i<5;i++){
			let div = document.createElement('li')
			div.setAttribute('class','item')
			div.innerText = 6666
			app.appendChild(div)
		}
```

使用DocumentFragment一次性添加，回流一次：

```js
                let app = document.querySelector('.app')
		let fragement = document.createDocumentFragment()
		for(let i = 0;i<5;i++){
			let div = document.createElement('li')
			div.setAttribute('class','item')
			div.innerText = 6666
			fragement.appendChild(div)
		}
		app.appendChild(fragement)
```

总结：DocumentFragment节点不属于文档树，存在于内存中，并不在DOM中，所以将子元素插入到文档片段中时不会引起页面回流，因此使用DocumentFragment可以起到性能优化的作用。