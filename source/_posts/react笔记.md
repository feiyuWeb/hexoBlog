---
title: react笔记
date: 2019-02-21 22:29:14
tags:
---

React 是一个为数据提供渲染为 HTML 视图的开源 JavaScript 库。React 视图通常采用包含以自定义 HTML 标记规定的其他组件的组件渲染。React 为程序员提供了一种子组件不能直接影响外层组件的模型，数据改变时对 HTML 文档的有效更新，和现代单页应用中组件之间干净的分离。

> 可以使用`jsx`中的点表示来引用`react`组件
> 例如：MyComponents.DatePiker 的组件，可以直接在 jsx 中使用它

```js
import React from "react"
const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Image a {props.color} here</div>
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />
}
```

**用户自定义组件的首字母必须大写**

```js
import React from "react"

// 正确！组件名应该首字母大写:
function Hello(props) {
  // 正确！div 是有效的 HTML 标签:
  return <div>Hello {props.toWhat}</div>
}

function HelloWorld() {
  // 正确！React 能够将大写开头的标签名认为是 React 组件。
  return <Hello toWhat="World" />
}
```

> 运行时选择类型

```js
import React from "react"
import { PhotoStory, VideoStory } from "./stories"

const components = {
  photo: PhotoStory,
  video: VideoStory
}

function Story(props) {
  // 正确！JSX 标签名可以为大写开头的变量。
  const SpecificStory = components[props.storyType]
  return <SpecificStory story={props.story} />
}
```

如果你已经有了个 props 对象，并且想在 JSX 中传递它，你可以使用 ... 作为“展开(spread)”操作符来传递整个属性对象。下面两个组件是等效的：

```js
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />
}

function App2() {
  const props = { firstName: "Ben", lastName: "Hector" }
  return <Greeting {...props} />
}
```

> 使用`PropTypes`进行类型检查

```js
import PropTypes from "prop-types"
class Greeting extends React.Component {
  render() {
    return <h1>hello,{this.props.name}</h1>
  }
}
Greeting.propTypes = {
  name: proptypes.string
}
```

_PropTypes 包含一整套验证器，可用于确保你接收的数据是有效的。在这个示例中，我们使用了 PropTypes.string。当你给属性传递了无效值时，JavsScript 控制台将会打印警告。出于性能原因，propTypes 只在开发模式下进行检查_

> 你可以通过配置 defaultProps 为 props 定义默认值：

```js
// 自执行函数
;(function(x) {
  console.log(x) // 10
})(10)
```

```js
class Greeting extends React.Component {
  render() {
    return <h1>hello,{this.props.name}</h1>
  }
}

// 为属性指定默认值
Greeting.defaultProps = {
  name: "Stranger"
}

// 渲染 'hello,Stranger'
ReactDOM.render(<Greeting />, document.getElementById("example"))
```

> Refs 提供了一种方式，用于访问在 render 方法中创建的 DOM 节点或 React 元素

```js
class MyComponent extends React.Component {
  constructor(props) {
    surper(props)
    this.myRef = React.createRef()
  }
  rebder() {
    return <div ref={this.myRft} />
  }
}
```

> redux 的使用

```js
// 定义reducer函数,名字可以随意起
function reducer(state,action) {
  /*初始化state和switch case*/
}

// 生成store
const store = createStore(reducer)

// 监听数据变化重新渲染页面
store.subscribe(()=>renderApp(store.getState())

// 获取state的值
store.getState()

// action只能用dispatch去触发更新
store.dispatch({type:'ADD'}) // 调用type为ADD的action
```

### State 的更新可能是异步的

> 因为 `this.props` 和 `this.state` 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。

```js
// 例如，此代码可能会无法更新计数器：
// wrong
this.setState({
  counter: this.state.counter + this.props.increment
})
```

> 要解决这个问题，可以让 `setState()` 接收一个函数而不是一个对象。这个函数用上一个 `state` 作为第一个参数，将此次更新被应用时的 `props` 做为第二个参数

```js
// correct  正确的方法去更新下一个状态
this.setState((state, props) => ({
  counter: state.counter + props.increment
}))
// 这两种写法效果是一致的，在箭头函数后加({}),相当于return{}
this.setState((state, props) => {
  return { counter: state.counter + props.increment }
})
```

### 向事件处理程序传递参数

> 在循环中，通常我们会为事件处理函数传递额外的参数。例如，若 `id` 是你要删除那一行的 ID，以下两种方式都可以向事件处理函数传递参数：

```jsx
<button onClick = {(e)=>this.deleRow(id,e)}> Delete Row </button>
<button onClick = {this.deleteRow.bind(this,id)}> Delete Row </button>
```

> 我们建议这些组件使用一个特殊的 `children prop` 来将他们的子组件传递到渲染结果中：

```jsx
function FancyBorder(props) {
  return (
    <div className={"FancyBorder FancyBorder-" + props.color}>
      {props.children}
    </div>
  )
}
```

> 这使得别的组件可以通过 JSX 嵌套，将任意组件作为子组件传递给它们。

```js
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">Welcome</h1>
      <p className="Dialog-message">Thank you for visiting our spacecraft!</p>
    </FancyBorder>
  )
}
```

> 在 React 中，我们也可以通过组合来实现这一点。“特殊”组件可以通过 props 定制并渲染“一般”组件：

```js
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">{props.title}</h1>
      <p className="Dialog-message">{props.message}</p>
    </FancyBorder>
  )
}

function WelcomeDialog() {
  return (
    <Dialog title="Welcome" message="Thank you for visiting our spacecraft!" />
  )
}
```

> 在 `JavaScript class` 中，每次你定义其子类的构造函数时，都需要调用 `super` 方法。
> 因此，在所有含有构造函数的的 `React` 组件中，构造函数必须以 `super(props)` 开头。
