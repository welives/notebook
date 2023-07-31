<center><h1>Vue2响应式原理</h1></center>

> 首先要知道 Vue2 是2013年基于 ES5 开发出来的，我们常说的重渲染就是重新运行`render`函数
>
> Vue2 的响应式原理是利⽤ ES5 的⼀个API，`Object.defineProperty()`对数据进⾏劫持结合发布订阅模式的⽅式来实现的

