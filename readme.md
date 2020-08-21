# JSX语法是怎样解析的
JSX是javascript和XML结合的一种格式，React发明了JSX，利用HTML语法来创建虚拟DOM，当遇到"<"，JSX就当HTML解析，遇到“{”就当js解析。JSX语法的本质并不是把JSX渲染到页面，而是在内部先转换成了createElement形式，然后再去渲染，同时JSX在进行编译成js代码的时候进行了一定的优化，所以执行效率也更高。

在react环境里预装上babel和相关的插件，通过配置来进行语法解析 plugins: [["@babel/plugin-transform-React-jsx{pragma:"ToyReact.createElement" },]
@babel/plugin-transform-React-jsx 做了什么呢？
jsx代码：
```
<div>
<div></div>
<div></div>
<div></div>
</div>
```
转化之后代码:
```
React.createElement(
"div",
{},
React.createElement("div", {}, ...chidren),
React.createElement("div", {}, ...chidren),
React.createElement("div", {}, ...chidren)
)
```
分为5步
1. 创建tagNode变量
2. 创建ToyReact.createElement表达式
3. 创建attribs对象
4. 创建ToyReact.createElement('div', {}, ...children)表达式
5. 替换node

# ToyReact 生命周期内都发生了什么
组件将要挂载时触发的函数: componentWillMount
组件挂载完成是触发的函数: componentDidMount
是否要更新数据时触发的函数: shouldComponentUpdate
将要更新数据时触发的函数：componentWillUpdate
数据更新完成时触发：componentDidUpdate
组件销毁时触发：componentWillUnmount

其中每个组件render之前都会调用componentWillMount()，可以在服务端或者浏览器端调用，如果有异步请求，是不推荐在这个时候请求数据的，在组件将要更新数据的时候都会触发componentWillUpdate()执行更新操作

toyReact中的mountTo和update 两个函数分别处理的就是挂载之前需要的操作和更新的时候需要的操作。在挂载之前通过setAttribute添加自定义属性，addEventListener添加事件，就是执行render()。如果有更新操作，会在update()内通过对比更新的元素进行替换，然后再render()

# Range对象的简单了解和使用
Range对象表示文档的连续范围区域，就是高亮区，一个Range的开始点和结束点可以是任意的，也可以一样，常见的应用场景时富文本编辑器中

首先会创建一个range对象(createRange)，将指定节点的终点位置指定为Range对象所代表区域的起点位置(setStartAfter)；紧接着将指定的节点插入到某个Range对象所代表的区域中，插入位置为Range对象所代表区域的起点位置，如果该节点已经存在于页面中，该节点将被移动到Range对象代表的区域的起点处(insertNode)。

# 虚拟DOM以及使用场景
用js模拟一颗DOM数，放在浏览器内存中。当变更时对虚拟DOM使用diff算法进行新旧虚拟DOM比较，将变更放到变更队列中，最终值把变化的部分重新渲染，从而提供渲染效率

