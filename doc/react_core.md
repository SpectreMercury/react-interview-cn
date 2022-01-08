### **1. React 是什么?**
React是一个开源的前端Javascript工具库，主要用来构建用户界面，尤其是单页面应用。它被用来处理Web和移动应用中视觉层的问题。React是Facebook（现Meta)公司软件工程师Jordan Walke开发的，最早在2011应用在Facebook新闻引用上，并在2012年应用到Instagram上。

### **2. React 的主要特点是什么?**
React的主要特点分以下几类：

- 考虑到操作真实DOM的高昂开销，使用虚拟DOM来替代了真实DOM
- 支持服务端渲染(SSR - Server Side Rendering)
- 使用单一数据流或者称作数据双向绑定
- 使用 可复用/可组合的UI组件来构成视图
  
### **3. 什么是JSX?**
 
 JSX是一种基于ECMAScript的类XML语法扩展。JSX基本上只为`React.createElement()`方法提供语法糖，为我们提供Javascript的表现力及HTML风格的语法

 下面的例子中，包含文字在`<h1>`标签在Javascript的方法中返回并渲染
 ```
 Class App extends React.Component {
    render() {
        return(
            <div>
                <h1>{'Welcome to React world!'}</h1>
            </div>
        )
    }
 } 
```

### 4. React中Element和Component中的区别

Elements是用来描述一个你想插入在DOM或者其他组件中的纯对象。Elements可以包含其他Elements的props。创建一个Elements的消耗非常低，并且Elements一旦创建，就不会发生变异

```
const element = React.createElement(
  'div',
  {id: 'login-btn'},
  'Login'
)
```
上面代码返回一个对象
```
{
    type: 'div',
    props: {
        children: 'Login',
        id: 'login-btn'
    }
}
```
最终在`ReactDOM.render()`中展示如
```<div id='login-btn'>Login</div>```

然而，一个组件不能被多种不同方式声明。它可以作为`render()`方法中的一个class或者她可以被定义为一个方法。在其他的例子当中，他将props当作一个输入，然后返回一个JSX Tree作为输出
```
const Button = ({ onLogin }) =>
    <div id={'login-btn'} onClick={onLogin}>Login</div>
```
之后JSX则会转译为一个`React.createElement()`方法树
```
const Button = ({ onLogin }) => React.createElement(
  'div',
  { id: 'login-btn', onClick: onLogin },
  'Login'
)
```
### 5. 在React中如何生成一个组件

有两种方法我们可以在React中创建一个组件：

- 方法组件（Funtion Components) 这是最简单的方式去创建一个组件。这些是单纯的Javascript方法，其接受传入的参数并返回React Elements
  ```
  function Greeting({ message }) {
    return <h1>{`Hello, ${message}`}</h1>
  }
  ```
- 类组件（Class Components) 你可以用ES6的Class去定一一个组件。上面的方法，用类组件可以用下面的方式改写
  ```
  class Greeting extends React.Component {
    render() {
        return <h1>{`Hello, ${this.props.message}`}</h1>
    }
  }
  ```


### 6. 什么时候用Class Component 什么时候用Function Componentn？

如果你要创建的Component需要State或者生命周期，则用Class Component，其他时候用Function Component。 但是从React 16.8增加了Hook之后，你可以在Function Components中使用state，生命周期的方法和其他功能

### 7. 什么是PureComponents

`React.PureComonents` 和 `React.Component` 几乎一样，但是他为你处理了`shouldComponentUpdate()`方法。当Props或者State发生改变的时候 `PureComponent`会对Props和State进行一个浅比较（Shallow Comparison)。另一方面，组件不会用当前的Props和State与其他的Box进行比较。因此，无论什么时候`shouldComponent`被调用，组件都会按默认重新渲染


### 8. 什么是React中state

State是生命周期中一个对象，它包含了一些信息，这些信息可能会在组件的生命周期中发生改变。我们需要一直控制state的体积尽可能的小，同时需要最小化有状态组件的数量。

下面是一个message state的组件样例

```
class User extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      message: 'Welcome to React world'
    }
  }

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
      </div>
    )
  }
}
```

### 9. React中什么是props

Props是组件的入参。它是一个值或者是包含一系列值得Object，这些值在创建的时候使用类似于HTML标签属性的命名约定传递给组件。他是从父组件传递到子组件的数据。

Props存在的主要目的是提供一下组件功能
- 传递自定义数据到组件
- 监听state的改变
- 在组件的`render()`方法中使用`this.props.reactProp`

举例来说，创建一个包含`reactProp`的element`<Element reactProp={'1'}/>`
这个`reactProp`（或者是任何你想象出）的名字成为React上原生的对象属性，该对象已经存储在于使用React创建的所有组件上
```
props.reactProp
```

### 10. State和Prop的区别
Props和State都是Javascrit Object。他们都控制着可以影响渲染输出的信息，但是他们在组件的功能不同。Props类似函数传递参数给组件，然而state是组件内部实现管理，像是生命在方法内部的变量

### 11. 为什么我们不直接改变State中的值

如果你直接去修改state的值，他不会重新渲染组件

``` 
//Wrong
this.state.message = 'Hello world'
```
使用`setState()`方法取代直接修改state的值。他会安排更新组件内部State对象，当state改变的时候，组件会响应并重新渲染组件
```
this.setState({message: 'Hello world'})
```
注意：你可以直接赋值，在constructor中, 或用最新的javascript类中声明赋值对象

### 12. `setState()`回调函数的目的是什么

这个回调函数会在`setState()`结束并且组件已经重新渲染完的时候执行。从`setState()`改为异步更新后，这个回调函数用于后续的操作。

注意：推荐使用生命周期中的方法而不是这个回调函数
```
setState({ name: 'John' }, () => console.log('The name has updated and component re-rendered'))

```

### 13. HTML和React的事件处理不同

下面是一些HTML和React事件处理的主要不同：

- 在HTML中，事件处理的惯例使用小写字母来明明
  ```
  <button onclick='activateLasers()'>
  ```
  然而而在React当中，遵循驼峰的命名惯例
  ```
  <button onClick={activateLasers}>
  ```

- 在HTML中，可以用`return false`来组织默认行为
  ```
  <a href='#' onclick='console.log("The link was clicked."); return false;' />
  ```
  然而在React当中，你必须明确调用`preventDefault()`
  ```
  function handleClick(event) {
      event.preventDefault()
      console.log('The link was clicked')
  }
  ```
- 在HTML中，你调用方法需要添加（），然而在React中，你只需要使用方法名就可以

### 14. 在JSX的回调中，如何绑定方法或者事件

有三种方式可以达到这个目的：

 - 在Constructor中绑定：在Javascript的类中，默认情况下，方法不受约束。同样的事情适用于定义为类方法的 React 事件处理程序。通常我们在constructor中绑定。
   ```
   class Foo extends Component {
        constructor(props) {
            super(props);
            this.handleClick = this.handleClick.bind(this);
        }
        handleClick() {
            console.log('Click happened');
        }
        render() {
            return <button onClick={this.handleClick}>Click Me</button>;
        }
   }
   ```
- 公共类字段语法：如果你不喜欢用bind方法来绑定，那么你可以使用class fields语法用来正确的绑定回调
  ```
  handleClick = () => {
      console.log('this is:', this)
  }
  ```
  ```
  <button onClick={this.handleClick}>
    {'Click me'}
  </button>
  ```
- 在回调中的箭头函数，你可以在回调中直接使用箭头函数
  ```
  handleClick() {
    console.log('Click happened');
  }
  ```
  ```
  render() {
    return <button onClick={() => this.handleClick()}>Click Me</button>;
  }
  ```
注意：考虑到回调会作为props传递给子组件，这些组件会产生额外的重新渲染。在这些情况下，考虑到性能问题，推荐使用`.bind()`或者公共类字段方法

### 15. 如何传递参数到事件处理方法或者回调中？

你可以用箭头函数，可以在括号中添加事件处理方法和传递参数

```
<button onClick={() => this.handleClick(id)}>
```
这样调用方法等同于`.bind()`
```
<button onClick={this.handleClick.bind(this, id)} />
```
除了上面两种方法，你还可以将参数传递给定义为箭头函数的方法
```
<button onClick={this.handleClick(id)} />
handleClick = (id) => () => {
    console.log("Hello, your ticket number is", id)
};
```