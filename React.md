# React

All in JS.

## 介绍

我们前端的原生开发是命令式编程，而 React 和 Vue 这些框架来开发是声明式编程。

React 是一个声明式，高效且灵活的用于构建用户界面的 JavaScript 库。

## 安装 和 使用

React 开发必须依赖三个库：

- react：react 的核心代码
- react-dom：react 渲染在不同平台所需要的核心代码
- babel：将 jsx 转成 react 代码

之所以要将源代码分为三个库，是因为 react-native。react-dom 针对 web 和 native 所完成的事情不同：

- web 端：react-dom 会将 jsx 渲染为 DOM
- native 端：将 jsx 渲染为原生控件

## JSX

JSX 是我们在开发 React 时，很常用的一种语法，是将 html 代码和 JS 代码混合的一种写法，我们需要 babel 来编译 JSX 语法。

看起来一个 JSX 是一个 HTML 标签，但是事实上并不是一个标签，而是会被编译为一个函数调用（`React.createElement()`），来创建一个 React 虚拟节点（一个对象）。

**书写规范**：

- 顶层只能有一个根元素。
- 为了方便阅读，通常在 JSX 外面包裹一个小括号。
- JSX 中可以写双标签，也可以用自闭和标签。

**注释**：

在 JSX 中的注释比较特殊，需要这么写：

```jsx
(
    <div>
    	{/* 我是注释 */}
        <h2>Hello World!</h2>
    </div>
)
```

**嵌入变量**：

- 字符串、数字、数组在 JSX 嵌入都可以正常显示。
- null、undefined、布尔类型都会被忽略不显示。
- 对象不能作为 JSX 的子类（不能直接显示）。

**嵌入表达式**：

有时我们可能更多地使用表达式来进行嵌入。

- 运算符表达式：

  ```react
  <h2>{ this.state.firstName + '-' + this.state.lastName }</h2>
  ```

- 三元表达式：

  ```react
  <h2>{ this.state.isLogin ? "欢迎回来" : "请先登录" }</h2>
  ```

- 函数调用：

  ```react
  <h2>{ this.getFullName() }</h2>
  ```

**绑定属性**：

在 HTML 中，属性也是很重要的。

```react
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            title: '标题',
            imgUrl: 'http://xxxx',
            active: true
        }
    }
    
    render() {
        const { title, imgUrl, active } = this.state
        
        return (
            <div>
            	<h2 title={ title }></h2>
                <img src={ imgUrl } />
                {/* 如果想要添加类名需要使用className，和JS类似，同理，label的for属性需要写为htmlFor */}   
                <div className="box"></div>
                <label htmlFor="ipt"></label>
                {/* 动态类名 */}
                <div className={"box " +active ? 'active' : ''}></div>
                {/* 动态样式，如果有两个单词的需要小驼峰 */}
                <div style={{color: 'red', fontSizw: '20px'}}></div>
            </div>
        )
    }
}
```

**绑定事件**：

```react
class App extends React.Component {
    constructor(props) {
        super(props)
     	// 在构造器中绑定一下，就不需要重复绑定了   
        this.clickHandle = this.clickHandle.bind(this)
    }
    
    render() {
        return (
            <div>
                {/* 这个函数的this指向undefined，因为是react调用的回调函数，要想解决可以有以下几种方式 */}
            	<button onClick={this.clickHandle}>按钮</button>
                {/* 一：显示的绑定一下this */}
            	<button onClick={this.clickHandle.bind(this)}>按钮</button>
                {/* 二：在构造器里绑定一次就好了 */}
            	<button onClick={this.clickHandle}>按钮</button>
                {/* 三：使用箭头函数来调用（推荐） */}
            	<button onClick={() => {this.clickHandle()}}>按钮</button>
                {/* 四：在定义回调函数的时候就直接使用箭头函数 */}
            </div>
        )
    }
    
    clickHandle() {
        console.log('button is click!')
    }
    
    // 或者直接定义为箭头函数
    clickHandle = () => {
        // 箭头函数没有创建自己的this，只会从上层执行上下文继承this，所以指向实例对象
        console.log(this)
    }
}
```

**JSX 的本质**：

JSX 的本质是 React.createElement(component, props, ...children) 的函数的语法糖。所有的 JSX 最后都会被 babel 转换为 React.createElement 的函数调用。

## 脚手架

在开发一个中大型项目时，我们需要使用打包工具来进行打包项目，但是可能需要我们学习 webpack、babel 等工具。为了避免学习成本太大，我们可以使用脚手架工具来快速搭建一个项目的环境，让我们的项目从搭建到开发，再到部署，变得更简单。

React 的脚手架叫做 create-react-app。

```shell
# 安装cli
npm i -g create-react-app
# 安装yarn
npm i -g yarn
```

**创建项目**：

```shell
create-react-app 项目名
```

### PWA

PWA 是渐进式 Web 开发。一个 PWA 应用首先是一个网页，可以通过 Web 技术编写出一个网页应用，随后添加上 `App Manifest` 和 `Service worker` 来实现 PWA 的安装和离线等功能。

PWA 可以添加至主屏幕，点击主屏幕图标即可实现启动动画以及隐藏地址栏。可以实现离线缓存的功能，即使用户手机没有网络也可以使用一些离线功能。实现了消息推送，等等一系列类似于 Native App 相关的功能。

我们的 React 项目搭建完，就默认创建了 `manifest.json` 和 `serviceWorker.js` 这两个文件，`manifest.json` 保存了一些配置信息，`serviceWorker.js`（4.x 不再生成）是一些实现了的函数接口，可以调用这些函数来实现 PWA 功能。如果不希望实现 PWA 相关的功能，可以不引入 `serviceWorker.js`（不引入打包就不会打包他），或者直接把这两个文件删掉。

### Webpack

脚手架将 webpack 的配置隐藏起来了，如果我们希望看到 webpack 的配置信息，我们可以执行 `eject` 这个脚本（eject 是喷出的意思），这个操作是不可逆的。

喷出配置后，会多出两个文件夹，一个是 config 文件夹，一个是 scripts 文件夹。

## 虚拟 DOM

在 React 中，我们使用 `React.createElement()` 来创建一个 `ReactElement` 对象。这个对象其实就是一个虚拟节点（Virtual Node），React 使用许多虚拟节点来组成一个虚拟 DOM 树。

**为什么使用虚拟 DOM？**

因为希望能追踪状态变化，而且 DOM 操作很浪费资源。所以我们先使用虚拟 DOM 进行对比，再进行更新。真实 DOM 节点太过复杂，而且 DOM 的改变会引起浏览器的回流和重绘，所以使用虚拟 DOM 来进行缓存对比。其实是使用 JS 的运算时间来换取操作 DOM 的开销。通过虚拟 DOM，让我们的编程方式从命令式编程转到了声明时编程。



