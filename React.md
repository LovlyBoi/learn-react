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
                <div className={"box " + (active ? 'active' : '')}></div>
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

## 组件化开发

组件化是 React 开发的核心思想，任何复杂应用最后都会被拆分为一个组件树。

React 组件相对于 Vue 更加的灵活，可以分为不同的组件：

- 根据组件的定义方式，可以分为：**函数组件** 和 **类组价**。
- 根据组件内部是否有状态需要维护，可以分为：**无状态组件** 和 **有状态组件**。
- 根据组件不同的职责，可以分为：**展示型组件** 和 **容器型组件**。

### 类组件

类组件有以下要求：

- 组件名称必须是大写字符开头（无论是类组件还是函数组件）；
- 类组件需要继承自 `React.Component`；
- 类组件**必须**实现 `render` 函数；

使用 class 来定义一个类组件，构造函数可选，`this.state` 中维护的就是组件的内部数据，`render()` 是类组件中**唯一必须实现**的方法。

```react
class App extends React.Component {
	render() {
		return (
      <div>
      	<h2>App Component</h2>
      </div>
    )
  }
}
```

`rander()` 函数的返回值必须是一下几种：

- React 元素，通常由 JSX 创建；
- 数组或 Fragment，使得 JSX 返回多个元素；
- 字符串或数字，会被渲染为文本节点；
- 布尔值或 null，什么都不渲染；
- Portals，可以渲染子节点到不同的 DOM 子树中；

### 函数组件

函数组件返回的内容和类组件中的 `render` 函数类似。

- 函数式组件中**没有 this 对象（组件实例）**；
- 函数式组件**没有内部状态**；
- 函数式组件**没有生命周期**，也会被更新挂载，但是没有生命周期函数；

```react
function App() {
	return (
      <div>
      	<h2>App Component</h2>
      </div>
    )
}
```

### 生命周期  

React 组件（类组件）有自己的生命周期，常用的有：

- 构造 constructor：组件被构造函数创建。
- 渲染 render：组件生成对应的 React 元素（创建或更新都会被调用）。
- 挂载 Mount：组件第一次在 DOM 中被渲染，调用 `componnetDidMount()` 函数。
- 更新 Update：组件状态变化，被重新渲染，调用 `componnetDidUpdate(prevProps, prevState, snapshot)` 函数，如果实现了 `getSnapshotBeforeUpdate` 方法，那么它的返回值会作为第三个参数传入。
- 卸载 UnMount：组件从 DOM 中被移除，调用 `componentWillUnmount()` 函数。

不太常用的有：

- `shouldComponentUpdate(nextProps, nextState)`：在传入的值更新时，默认会重新渲染组件，我们可以自己实现这个函数来控制是否重新渲染，返回 `true` 代表需要重新渲染，返回 `false` 则表示不需要重新渲染，不需要重新渲染更新也就意味着 `componnetDidUpdate` 不会被调用。
- `getSnapshotBeforeUpdate(prevProps, prevSate)`：在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。

我们可以实现这些生命周期函数来让组件在特定的阶段来做一些事情。

### 组件的通信

在开发中，我们肯定会对组件进行嵌套，这就引发了一个问题，不同的组件之间如何通信？

**父子组件间通信**：

- 父组件向子组件传递 props，来完成 父 -> 子 的通信。
- 父组件向子组件传递函数，子组件调用，来完成子 -> 父 的通信。

```react
import React, { Component } from 'react'

// 类组件
class Child extends Component {
  render() {
    return (
      <div>
        <h2>Son Component</h2>
        <h3>name: {this.props.name}</h3>
        <h3>age: {this.props.age}</h3>
        <button
          // 这里使用箭头函数包裹是因为回调函数this是undefined
          onClick={() => {
            // 点击时调用父组件传过来的处理函数
            this.props.handleChange()
          }}
        >
          change
        </button>
      </div>
    )
  }
}

// 函数组件
function Child(props) {
  return (
    <div>
      <h2>Son Component</h2>
      <h3>name: {props.name}</h3>
      <h3>age: {props.age}</h3>
      {/* 函数式组件就不需要箭头函数来找props了 */}
      <button onClick={props.handleChange}>change</button>
    </div>
  )
}

export default class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      name: 'Siven',
      age: 18,
    }
  }

  render() {
    const { name, age } = this.state
    return (
      <div>
        <Child
          name={name}
          age={age}
          // 这里使用箭头函数是因为希望保存下来this指向，希望this依旧指向父组件实例
          handleChange={() => {
            this.handleChange()
          }}
        />
      </div>
    )
  }

  // 传递给子组件的处理函数
  handleChange() {
    this.setState({
      name: 'Jobs',
      age: 56,
    })
  }
}

```

**属性验证**：

很多时候，我们在传值时需要确定传入 prop 的类型，我们需要一个单独的库（prop-types）来帮助我们完成（当然如果我们用了  TS 就不用了）。

```react
// 引入PropTypes库
import PropTypes from 'prop-types'

function Child(props) {
  return (
    <div>
      <h2>Son Component</h2>
      <h3>name: {props.name}</h3>
      <h3>age: {props.age}</h3>
      <button onClick={props.handleChange}>change</button>
    </div>
  )
}

// 定义prop类型
Child.propTypes = {
  // name必须是string类型
  name: PropTypes.string,
  age: PropTypes.number,
  // 处理函数handleChange必须是函数类型
  handleChange: PropTypes.func
}

// 默认值
Child.defaultProps = {
  name: 'Siven',
  age: 18,
  handleChange: () => {}
}

// 或者我们把它写进类里面
class Child extends Component {
  // 类属性
  static propTypes = {
    // name必须是string类型
    name: PropTypes.string,
    age: PropTypes.number,
    // 处理函数handleChange必须是函数类型
    handleChange: PropTypes.func
  }
  
  // ...
}
```

**跨组件通信**：

如果我们的组件跨了一层以上，我们就不太好直接传递了，而是会通过中间组件传递，但是这对于中间组件来说有些冗余：中间组件并不需要这些数据，却要在这里负责逐层传递。

这时我们可以使用 context，来避免逐层传递数据。

相关的 API 有：

- `React.createContext` 创建一个需要共享的 context 对象，如果一个组件订阅了 context，那么这个组件会从离自身最近的那个匹配的 Provider 中读取当前的 context 值，defaultValue 是组件在顶层查找过程中没有找到对应的 Provider，那么就使用默认值。
- `Context.Provider` 每个 Context 对象都会返回一个 Provider React 组件，他允许消费组件订阅 context 的变化，Provider 接受一个 value 属性，传递给消费组件。一个 Provider 可以和多个消费组件有对应关系，多个 Provider 也可以嵌套使用，里层的数据会覆盖外层的数据。当 Provider 的 value 发生变化时，它内部的所有消费组件都会重新渲染。
- `contextType` 挂载在 class 上的 contextType 属性会被重新赋值为一个由 React.createContext() 创建的 Context 对象，这能让我们通过 this.context 来消费最近的 Context 上的那个值，可以在任何生命周期，以及 render 函数中访问。
- `Context.Consumer` 这里，React 组件也可以订阅到 context 的变更，这能让我们在 函数组件 中完成订阅 context，这里需要函数作为子元素这种做法，这个函数接收当前的 context 值，返回一个 React 节点。

```react
import React, { Component } from 'react'

// 创建Context对象，这里传过来的是默认数据
const UserContext = React.createContext({
  name: 'Jobs',
  age: 56,
})

// 如果是一个函数组件，需要函数作为子元素，返回一个React节点
function ChildComponent(props) {
  return (
    // 需要把外面改为UserContext.Consumer，表示是一个消费者
    <UserContext.Consumer className="child-cpn">
      {(value) => {
        return (
          <div>
            <h2>name: {value.name}</h2>
            <h2>age: {value.age}</h2>
          </div>
        )
      }}
    </UserContext.Consumer>
  )
}

// 这种写法需要子组件是一个类组件
class ChildComponent extends Component {

  // 需要指定contextType属性
  static contextType = UserContext

  render() {
    return (
      <h2 className="child-cpn">
        name: {this.context.name}, age: {this.context.age}
      </h2>
    )
  }
}

function MiddleComponent(props) {
  return (
    <div className="middle-cpn">
      <ChildComponent />
    </div>
  )
}

export default class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      name: 'Siven',
      age: 18,
    }
  }

  // 我们希望把数据从App传递到ChildComponent中
  render() {
    return (
      <div className="app">
        {/* 使用Provider组件，传入数据 */}
        <UserContext.Provider value={this.state}>
          {/* 让需要拿到数据的组件是Provider的子孙组件 */}
          <MiddleComponent />
        </UserContext.Provider>
      </div>
    )
  }
}

```

因为这么写代码较为冗余，所以我们开发中使用更多的还是 redux 和 hooks。

**全局事件总线**：

事件总线（event bus）实质上是一个全局对象，我们可以通过它来发出事件，也可以通过它监听一些事件。

在 React 中我们使用比较多的是 events 这个第三方库。

```shell
npm i events
```

```react
import React, { PureComponent } from 'react'
import { EventEmmiter } from 'events'

// 产生一个eventBus
const eventBus = new EventEmitter()

// 一个想要发出事件的组件
class EmitterCpn extends PureComponent {
    render() {}
    
    function emitHello() {
		// 第一个参数为事件名，后面的参数是携带的数据
        eventBus.emit('sayHello', 'Hello World!')
    }
}

// 事件监听
class AcceptEventCpn extends PureComponent {
    // 在挂载后添加事件监听
    componentDidMount() {
        eventBus.addListener('sayHello', this.handleSayHello)
    }
    
    // 卸载前取消监听
    componentWillUnmount() {
        eventBus.removeListener('sayHello', this.handleSayHello)
    }
    
    // 处理函数
    handleSayHello() {
        
    }
	render() {}
}
```



## setState

我们不能直接修改 state 中的数据，而是需要通过 setState 来对 state 修改，因为直接修改 state，React 是检测不到的（React 没有像 Vue 检测 data 那样的响应式系统）。setState 是 Component 组件实现的方法。

### 异步更新

setState 是异步更新的（事实上不是异步的，而是收集起来批处理了），所以如果同步的读取 state 中的数据，那么数据肯定是未修改的。

那么为什么要把 setState 设计为异步更新的呢？

因为 setState 设计为异步，可以显著提高性能。如果每一次使用 setState 都需要同步更新，那么就意味着 render 函数被反复调用，界面反复重新渲染，而异步更新意味着可以批量更新。并且，如果同步更新了 state，但是还没有执行 render 函数，那么 state 和 props 不能保持同步。

如果希望得到异步的数据，需要传入 setState 的第二个参数，是一个回调函数，这个回调函数会在数据更新完成后被回调（类似于 Vue 中的 nextTick）。

### 同步更新

有时，setState 却是同步的更新：

- 在 setTimeout 回调中的更新是**同步**的
- 在原生 DOM 事件回调中，也是**同步**的
- 在组件生命周期或 React 合成事件中，setState 是**异步**的

### 数据合并

在 setState 更新时，并不会直接使用新的数据完全覆盖旧数据，而是使用 `Object.assign({}, 原来的state, 新的state)` 来进行一个浅拷贝，所以不需要担心会覆盖其他的数据。

### setState 合并

多次调用 setState 会导致这些操作被合并为一次 setState，如果不希望被合并后导致多次调用后只生效一次，可以选择将第一个参数传入一个函数：

```js
// 这里prevState是上一次的state
this.setState((prevState, props) => {
  return {
    counter: prevState.counter + 1
  }
})

this.setState((prevState, props) => {
  return {
    counter: prevState.counter + 1
  }
})

// 调用2次，就会加2，如果是正常的传入对象的setState，则会被合并为最后一次操作
```

### setState 数据不可变

如果我们直接修改了 state，又调用 setState 更新状态，那么我们就违反了数据不可变原则。

这样做虽然可以正常显示，但是就无法进行 SCU 优化，因为传进来的 nextState 和 之前的 prevState 是一样的（因为我们直接修改了 state）。

## React 更新流程

props/state 改变 --> render 函数重新执行 --> 生成新的虚拟 DOM --> 新旧 DOM 进行 diff --> 找到差异更新 --> 得到新的真实 DOM

### 虚拟 DOM

在 React 中，我们使用 `React.createElement()` 来创建一个 `ReactElement` 对象。这个对象其实就是一个虚拟节点（Virtual Node），React 使用许多虚拟节点来组成一个虚拟 DOM 树。

**为什么使用虚拟 DOM？**

因为希望能追踪状态变化，而且 DOM 操作很浪费资源。所以我们先使用虚拟 DOM 进行对比，再进行更新。真实 DOM 节点太过复杂，而且 DOM 的改变会引起浏览器的回流和重绘，所以使用虚拟 DOM 来进行缓存对比。其实是使用 JS 的运算时间来换取操作 DOM 的开销（减少 DOM 操作）。通过虚拟 DOM，让我们的编程方式从命令式编程转到了声明时编程。使用虚拟 DOM 还可以使应用具有跨平台性。

### Diff 算法

如果对两棵树完全进行对比更新，那么时间复杂度是 O(n^3)，这对于 React 来说开销太昂贵了。

所以 React 将这个算法简化到了 O(n) 级别：

-  只有同级别的节点之间才会进行比较，跨级节点并不会进行比较。
- 只要节点类型不一样，整颗子树都重新渲染。
- 可以通过 key 来指定哪些结点在不同的渲染下保持稳定。
- 节点类型相同，保留 DOM 元素，仅对比更新改变的属性。
- 如果组件类型相同，更新该组件的 props，调用 render 方法，对新旧 DOM 接着递归 diff。

可以再看看这篇文章：[React、Vue2、Vue3的三种Diff算法](https://juejin.cn/post/6919376064833667080#heading-0 )。

**keys 优化**：

列表渲染中，仅仅在中间插入了一条数据，但是由于新旧 DOM diff 时依次对比，发现从插入的数据开始全部都不同，导致后面所有的节点都需要重新渲染。

为了防止这种情况，我们可以给每一个节点配置一个 key 值，来标识每一个节点，让他们可以被识别出来，这样我们就可以知道哪些元素只需要位移，而不是重新删除再创建。

作为 key，不要使用每次渲染都会改变的值，这样做对于性能是没有优化的，一般来说都是使用 id。

```react
<ul>
	{
    this.state.movies.map((item) => {
      // 给每一个子元素添加一个单独的key
      return (<li key={item}></li>)
    })
  }
</ul>
```

## React 性能优化

每次重新执行 setState 之后，render 函数被重新调用，但是有时一个较高级组件的 setState 被重新调用之后，会让所有低一级的组件全部重新调用 render 函数，这有可能是完全没有必要的，因为可能只有其中一个低级组件需要重新 render。

### SCU 优化

我们可以通过 `shouldComponentUpdate` 这个函数来阻止一些不必要的 render。当这个函数返回 false 时，就可以阻止这个组件更新。

```react
// 传进来新的props和state
shouldComponentUpdate(nextProps, nextState) {
	if(this.state.counter !== nextState.counter) {
        // 只有新的state中的counter改变时才会重新渲染
        return true
    }
    return false
}
```

### PureComponent

如果不想一个一个实现 `shouldComponentUpdate`，只需要让组件继承自 `PureComponent` 类。

渲染时会调用 `checkShouldComponentUpdate` 函数来决定组件是否更新，如果检测到了我们自己实现了 `shouldComponentUpdate` 方法，会根据我们自己的方法来决定组件是否更新， `checkShouldComponentUpdate` 默认返回 true，所以组价默认会更新。

而如果组件是一个 `PureComponent` 类，则会对新旧 state 和 props 进行一个**浅比较**（shallowEqual），如果有不同，则会返回 true 来让组件更新，否则返回 false 阻止更新。

### memo

但是上面两种方法只能解决类组件，解决不了函数组件被调用的问题。对于函数组件，我们可以使用 memo 来解决：

```react
import React, { memo } from 'react'

// 返回一个组件，我们就可以使用这个返回的组件
const MemoHeader = memo(function Header(props) {
    return <h2>msg: {props.msg}</h2>
})

```

开发时我们的函数组件都可以包裹一个 memo。

memo 就是对组件进行了一次包裹，返回的对象多出了一个 compare 属性，如果没有自己实现 compare，这个 compare 就是一个 shallowEqual，后面的实现本质就基本一样了。

## Ref

在 React 开发过程中，通常情况下不需要，也就建议直接操作 DOM，但是某些情况下，确实需要获取 DOM 进行某些操作：

- 管理焦点，文本选择或媒体播放
- 触发强制动画
- 继承第三方 DOM 库

### 使用 Ref

如何创建 refs 来获取 DOM 元素？

- 传入字符串。使用时通过 `this.refs.xxx` 来获取对应的元素
- 传入一个对象。对象通过 `React.createRef()` 创建，使用时获取到创建的对象中的 `crrent` 属性就是对应元素
- 传入一个函数。该函数会在 DOM 被挂载的时候回调，这个函数会传入一个 元素对象，我们可以自己保存，使用时直接拿到之前保存的元素对象即可。

 

```react
import React, { PureComponent, createRef } from 'react'

class App extends PureComponent {
  constructor(props) {
		super(props)
    // 创建引用对象
    this.titleRef2 = createRef()
    // 回调函数中赋值给这个变量
    this.titleRef3 = null
  }
  
  render() {
    return (
    	<div>
        {/* 第一种方式 */}
        {/* 通过ref属性告诉React这是哪一个引用，要获取他的元素的引用，即虚拟元素的ref值 */}
      	<h2 ref="titleRef">Hello World!</h2>
        
        {/* 第二种方式 */}
        {/* 通过createRef函数创建一个对象，将对象传进ref属性，这是React推荐的方式 */}
        <h2 ref={titleRef2}>Hello World!</h2> 
        
        {/* 第三种方式 */}
        {/* 传入一个回调函数，参数是这个DOM元素 */}
        <h2 ref={arg => {this.titleRef3 = arg }}>Hello World!</h2> 
        
        <button onClick={e => {this.handleClick()}}></button>
      </div>
    )
  }
  
  handleClick() {
    // 使用方式一：可以在这里直接操作DOM元素（不推荐）
		this.refs.titleRef.innerHTML = "Hello React!"
    // 使用方式二：直接从组件实例中拿到引用，是一个对象，current属性是元素本身（React推荐）
    this.titleRef2.current.innerHTML = "Hello React!"
    // 使用方式三：回调函数中赋值的变量就是DOM元素
    this.titleRef3.innerHTML = "Hello React!"
  }
}
```

### Ref 的类型

ref 的值根据节点的类型而有所不同：

- 当 ref 引用的是 HTML 元素时，构造函数中使用 React.createRef() 创建的 ref 接收底层 DOM 元素作为 `current` 属性。
- 当 ref 引用的是 组件 时，ref 只想对应的组件实例作为 `current` 属性。
- **不能在函数组件上使用 ref，因为他们没有实例**。

因为 Ref 能获取组件实例，所以可以直接操作组件的方法。

## 受控组件和非受控组件

### 受控组件

在 React 中，表单的处理方式和普通 DOM 不太一样，表单元素通常会保存一些内部的 state。

比如一个 form 表单，我们通常使用 JS 来处理提交，而不是选择 HTML 的默认行为（React 没有阻止表单提交的默认行为），实现这种效果的标准方式是受控组价。 

在 React 中，可变状态一般保存在 state 中， 并只能通过 setState 来更新。我们使 state 作为唯一数据源，渲染表单的 React 元素还控制着输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做受控组件。 

```react
import React, { PureComponent } from 'react'

class App extends PureComponent {
  constructor(props) {
    super(props)
    this.state = {
      username: '',
      options: 'option1'
    }
  }
  
  render() {
    return (
    	<form onSubmit={e => {this.handleSubmit(e)}}>
        {/* input */}
      	<label htmlFor="username">
        	<input type="value" 
            		 id="username" 
            		 onChange={e => this.handleInputChange(e, 'username')}
            		 value={this.state.username}
          ></input>
        </label>
        
        {/* select */}
        <select name="options" 
          			value={this.state.options}
          			onChange={e => this.handleSelectChange(e, 'options')}
        >
        	<option value="option1">option1</option>
          <option value="option2">option2</option>
          <option value="option3">option3</option>
        </select>
        
        <input type="submit" value="提交" />
      </form>
    )
  }
  
  handleSubmit(event) {
    event.preventDefault()
    // 拿到数据
    console.log(this.state)
  }
  
  handleChange(event, key) {
    this.setState({
      // 不需要写很多函数
      [key]: event.target.value
      // 或者直接用event.target.name来做key
      [event.target.name]: event.target.value
    })
  }
}
```

### 非受控组件

React 推荐尽量使用受控组件来处理表单数据。在一个受控组件中，表单数据是由 React 来管理的，另一种方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。如果使用非受控组件，那么我们需要使用 ref 来获取数据。
