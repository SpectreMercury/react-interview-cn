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


### 6. 什么时候用Class Component 什么时候用Function Component？

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

### 16.什么React中的合成事件

`SyntheticEvent`是React模拟原生DOM事件所有能力的一个事件对象，即浏览器原生事件的跨浏览器包装器。他的API和浏览器的原生事件相同，包含`stopPropagtion()`和`preventDefault()`，除了事件在所有浏览器中的工作方式相同


### 17.什么是内联条件表达式

你可以使用Javascript中支持渲染的if表达式或者三元表达式。除了这些外，你还可以在JSX中嵌入任何表达式，把他们包裹在`{}`中，之后你可以遵循Javascript的逻辑，使用`&&`等

```
<h1>Hello!</h1>
{
    messages.length > 0 && !isLogin?
      <h2>
          You have {messages.length} unread messages.
      </h2>
      :
      <h2>
          You don't have unread messages.
      </h2>
}
```

### 18. 什么是`key` prop，在使用数组渲染elements中，他有什么优势？
在用数组创建elements的时候，`key`是一个特特殊的字符串属性。`Key` prop 帮助React识别哪个子元素发生了改变，添加或者移除。
通常我们用`ID`当做`key`
```
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
)
```
当你没有稳定的`ID`来渲染子元素的时候，你可以用子元素的`index`来当做key作为最后的手段
```
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
)shao
```
注意：

- 如何子元素会变化的话，通常不推荐使用`Index`来当做`key`。因为这回对框架的渲染表现造成负面的影响，同时也会造成组件内`State`的问题

- 如果组件是按照list来分割，更新的，那么把key应用到list的组件上，而不是用在`li`标签上

- 如果在循环中没有`key`的话，在浏览器中会有warning出现

### 19.refs在React中的作用？

ref用来返回一个elements的reference。在多数情况下，他应该被避免使用，但当你想要直接访问一个DOM元素或者组件实例的情况下，他会体现出作用。

### 20. 如何创建refs？

有两种方法可以创建refs

- 这是一个最近添加的方法， Refs用`React.createRef()`方法，然后直接在React的Elements上注册`ref`属性的方法。想要传递refs到组件当中，只需要在构造函数里赋值ref给实例即可
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.myRef = React.createRef()
  }
  render() {
    return <div ref={this.myRef} />
  }
}
```
- 无论你使用哪个版本的React，你都可以使用Ref的回调方法。举例来说，如下方式访问搜索栏组件input elements
```
class SearchBar extends Component {
   constructor(props) {
      super(props);
      this.txtSearch = null;
      this.state = { term: '' };
      this.setInputSearchRef = e => {
         this.txtSearch = e;
      }
   }
   onInputChange(event) {
      this.setState({ term: this.txtSearch.value });
   }
   render() {
      return (
         <input
            value={this.state.term}
            onChange={this.onInputChange.bind(this)}
            ref={this.setInputSearchRef} />
      );
   }
}
```
- 你还可以使用`closures`在方法组件中使用refs.

### 21. 什么是Forward Refs

Ref 转发是一项功能，它允许某些组件获取它们接收到的 ref，并将其进一步传递给子组件。
```
const ButtonElement = React.forwardRef((props, ref) => (
  <button ref={ref} className="CustomButton">
    {props.children}
  </button>
));

// Create ref to the DOM button:
const ref = React.createRef();
<ButtonElement ref={ref}>{'Forward Ref'}</ButtonElement>
```

### 22.refs和findDOMNode()哪个是回调的首选项

首选肯定是refs而不是findDOMNode(), 因为findDOMNode()阻碍了React在将来的一些更新和发展。

传统的findDOMNode方法:
```
class MyComponent extends Component {
  componentDidMount() {
    findDOMNode(this).scrollIntoView()
  }

  render() {
    return <div />
  }
}
```
推荐的使用方法
```
class MyComponent extends Component {
  constructor(props){
    super(props);
    this.node = createRef();
  }
  componentDidMount() {
    this.node.current.scrollIntoView();
  }

  render() {
    return <div ref={this.node} />
  }
}
```

### 23.为什么说字符串的refs是传统的？

如果你之前使用过React，你一定特别熟悉字符串API版本的`ref`, 像`ref={'textInput'}`, 可以使用`this.ref.textInput`访问DOM。我们不推荐使用这样的访问方式，考虑到传统的方式，它有以下的问题。字符串Ref在React v16中已经被移除了

- 他们迫使React必须追踪当前执行的组件。这样是有问题的，他使react的模块有状态，这样当包里的模块重复时，他会产生奇怪的错误
- 他不是可组合的 - 如果一个库传递一个ref到他的子组件，用户就不能传递其他的ref过去。回调的模式则可以做的完美的ref组合
- 它不适用于像Flow那样的静态分析。Flow 无法猜测框架使字符串 ref 出现在 this.refs 上的魔法，以及它的类型（可能不同）。 回调引用对静态分析更友好。
- 他不能像多数人期待的那样渲染回调的方式运行
  ```
  class MyComponent extends Component {
    renderRow = (index) => {
      // This won't work. Ref will get attached to DataTable rather than MyComponent:
      return <input ref={'input-' + index} />;

      // This would work though! Callback refs are awesome.
      return <input ref={input => this['input-' + index] = input} />;
    }

    render() {
      return <DataTable data={this.props.data} renderRow={this.renderRow} />
    }
  }
  ```

### 24. 什么是虚拟DOM？

虚拟DOM是真是DOM在内存中的表示。UI的表示在内存当中，并与真实的DOM同步。这一步是UI在视觉上的表现保存在内存当中和展示元素在屏幕上的一步。这整个过程称之为一致性

### 25.虚拟DOM是如何工作的？
虚拟DOM的工作原理基本上分三步：

- 无论什么时候下划线的数据发生改变，整个UI都会在虚拟DOM中重新渲染
  ![vm-work](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/vdom1.png)

- 计算当前DOM和新DOM的差异
  ![vm-work](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/vdom2.png)

- 计算结束后，更新真实的DOM，但是只有改变的部分会被更新
  ![vm-work](https://github.com/sudheerj/reactjs-interview-questions/raw/master/images/vdom3.png)

### 26. 影子DOM和虚拟DOM的区别

影子DOM是浏览器的技术，主要解决Web组件中的范围变量和CSS。虚拟DOM则是Javascript库在浏览器API上的实现的一个概念。

### 27. 什么是React Fiber架构?

Fiber是React v16版本新协调引擎活核心算法的重构。React Fiber主要的目的是提高在动画，布局，手势，暂停，中止或者重用工作的能力以及为不同类型的更新分配优先级等领域的适用性。

### 28. React Fiber的主要目的？

除了26描述的内容外，React Fiber能够将渲染的工作分成块并将其分散到各个帧当中。从文档来看，主要目标有：

- 能够在模块中分离中断的任务
- 能够对现在进行的工作的优先级进行排序，重新定位和复用
- 能够在父子组件之间进行让步来支持React的布局
- 支持`render()函数返回多个元素`
- 更好的支持边界错误

### 29. 什么是可控制的组件

一个组件控制用户在form表单的input元素中的操作叫作可控制组件，比如，每一个状态改变都有一个程序处理

比如说，输入所有的名字转换为大写
```
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()})
}
```

### 30. 什么是非控制组件

不受控制的组件就是组件内部自己存储组建的状态，你可以使用ref去查看dom当前的值，这点有点类似传统的HTML
```
class UserProfile extends React.Component {
  constructor(props) {
    super(props)
    this.handleSubmit = this.handleSubmit.bind(this)
    this.input = React.createRef()
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value)
    event.preventDefault()
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          {'Name:'}
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

### 31. `createElement`和`cloneElement`的区别是什么？

JSX的元素会被`React.createElement()`方法用来创建React元素来表现UI。然而`cloneElement`用来克隆一个element然后给他传递新的props

### 32. React中的状态提升是什么？

如果多个组件需要共享同样的数据变化，那么这种情况下推荐提升他们的共享状态到他们最近的祖先组件当中。就是说，如果两个子组件从同一个父组件而来共享同样的状态变化，那就把状态放到父组件中区维护，而不是在两个子组件中都维护一份

### 33. 组件的生命周期的区别

组件的声明周期有三个显著的生命周期状态：

- Mounting：组件已经准备好挂载在浏览器的DOM伤。这个阶段包括`constructor()`, `getDerivedStateFromProps()`,`render()`和`componentDidMount()`生命周期方法
- Updating：在这个阶段，组件更新有两种方式，一种是有新的props传递，另一种是使用`setState()`或者`forUpdate()`更新组件中的状态。这个剪短覆盖了`getDerviedStateFromProps()`, `shouldComponentnUpdate()`, `render()`, `geSnapshotBeforeUpdate()`和`componentDidUpdate()`
- Unmounting：在最后这个阶段，组件不再被需要，并且会从浏览器的DOM中取消挂在。这个阶段包含`componentWillUnmount()`生命周期方法

值得注意的是，在更新DOM的时候，React内部包含一个阶段性的概念。它分为以下几点：
- 渲染阶段，组件将会在没有任何副作用的时候渲染。这个阶段适用于pure component，在渲染阶段，她可以被暂停，抛弃或者重新渲染。
- 预提交状态(Pre-commit)：在组件真正提交改变到真实DOM之前，在这个阶段允许React通过`getSnapShotBeforeUpdate()`去读取DOM
- 提交改变 React通过与DOM合作，并分别执行生命周期的`componentDidMount()` 用于挂载，`componentDidUpdate()`用于更新，和`componentWillUnmount()`用于结束挂载
  
### 34. 什么是React中的声明周期？

- getDerivedStateFromProps： 在调用`render()`之前调用并在每次渲染时调用。 这适用于需要派生状态的罕见用例。
- componenntDidMount: 在第一次渲染之后执行，当所有的ajax请求，DOM或者状态更新和设置的事件监听都应该生效
- shouldComponentUpdate： 决定组件是否更新。默认情况下，其返回true。如果使用者确认状态和props的改变不用重新渲染该组件，可以返回false。这是一个很好的提高性能的方法，当组件收到新的prop的时候，可以允许你阻止重新渲染
- getSnapshotBeforeUpdate：在渲染输出到提交DOM之前执行，任何返回值都会被传递到`componenntDidUpdate()`。这对于从DOM捕获信息非常有用，比如（滚动）
- componentDidUpdate：通常，这个用作回应DOM根据prop和state的变化。如果`shouldComponentUpdate()`返回`false`，则该周期方法不会触发
- componentWillUnmount： 用来取消传出的网络请求或者移除与该组件相关的事件监听

### 35. 什么是高阶组件？

高阶组件是一个方法，他接收传入一个组件并返回一个新的组件。基本上他是源自React的一种组合特性。
我们称之为pure component，因为他们接受任何动态提供的子组件，但是他们不会对输入的组件有任何输入或者复制的行为。
```
const EnhancedComponent = higherOrderComponent(WrappedComponent)
```
高阶组件可以用在一下场景中：

- 代码复用，逻辑和引导抽象
- 渲染劫持
- 状态抽象和维护
- Props维护

### 36. 如何为高阶组件提供Props代理

你可以用props代理的模式修改/添加传递给组件的props
```
function HOC(WrappedComponent) {
  return class Test extends Component {
    render() {
      const newProps = {
        title: 'New Header',
        footer: false,
        showFeatureX: false,
        showFeatureY: true
      }

      return <WrappedComponent {...this.props} {...newProps} />
    }
  }
}
```

### 37. 什么事context？

Context提供了一种组件树之间传递数据的方法，同时还不用手动传递props。

或者例如，经过身份验证的用户、区域设置偏好、UI 主题需要在应用程序中被许多组件访问。

```
const {Provider, Consumer} = React.createContext(defaultValue)
```

### 38. 什么事子props？

子组件是prop(`this.props.children`)允许你像数据一样传递组件到其他组件，就像其他你使用的props一样。组件树会把组件开始和闭合标签之间的内容当做prop传递给子组件。

有一系列的React API可以和这个prop配合使用。包括`React.Children.map`, `React.Children.count`, `React.Children.only`,`React.Children.toArray`。

一个简单的children prop用例：
```
const MyDiv = React.createClass({
  render: function() {
    return <div>{this.props.children}</div>
  }
})

ReactDOM.render(
  <MyDiv>
    <span>{'Hello'}</span>
    <span>{'World'}</span>
  </MyDiv>,
  node
)
```

### 39. 如何在React中写注释
React/JSX 中的注释类似于 JavaScript 多行注释，但用花括号括起来。
单行注释:
```
<div>
  {/* Single-line comments(In vanilla JavaScript, the single-line comments are represented by double slash(//)) */}
  {`Welcome ${user}, let's play React`}
</div>
```
多行注释：
```
<div>
  {/* Multi-line comments for more than
   one line */}
  {`Welcome ${user}, let's play React`}
</div>
```

### 40. 在Constructor中使用super的目的？

在调用`super()`方法之前，子组件无法使用`this.props`，这同样适用于ES6的子类。将props参数传递给super的主要目的是可以在子构造函数中访问使用`this.props`使用

传递Props：
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    console.log(this.props) // prints { name: 'John', age: 42 }
  }
}
```
不使用props：
```
class MyComponent extends React.Component {
  constructor(props) {
    super()

    console.log(this.props) // prints undefined

    // but props parameter is still available
    console.log(props) // prints { name: 'John', age: 42 }
  }

  render() {
    // no difference outside constructor
    console.log(this.props) // prints { name: 'John', age: 42 }
  }
}
```
只是在constructor中不同，在constructor外面是一样的

### 41. 什么是一致性？

当组件的 props 或 state 发生变化时，React 通过比较新返回的元素和之前渲染的元素来决定是否需要进行实际的 DOM 更新。 当它们不相等时，React 将更新 DOM。 这个过程称为一致性。

### 42. 如何为state设置一个动态的key名

如果你在用ES6或者Babel转译JSX代码的话，你可以用计算属性名来完成这个需求

```
handleInputChange(event) {
  this.setState({ [event.target.id]: event.target.value })
}
```

### 43. React中常见的方法调用错误是什么？

```
render() {
  // Wrong: handleClick is called instead of passed as a reference!
  return <button onClick={this.handleClick()}>{'Click Me'}</button>
}
```
```
render() {
  // Correct: handleClick is passed as a reference!
  return <button onClick={this.handleClick}>{'Click Me'}</button>
}
```
### 44. 惰性函数是否支持命名导出

目前`React.lazy`函数只支持默认导出。如果你命名导出，你可以创建一个中间模块，将其导出为默认模块。它确保tree shaking继续工作不会拉取其他未使用的组件。
我们来看一下一次导出多个命名组件：
```
// MoreComponents.js
export const SomeComponent = /* ... */;
export const UnusedComponent = /* ... */;
```

在中间文件`IntermediateComponent.js`中重新导出`MoreComponents.js`组件
```
// IntermediateComponent.js
export { SomeComponent as default } from "./MoreComponents.js";
```
然后你就可以用下面的方法引入惰性函数
```
import React, { lazy } from 'react';
const SomeComponent = lazy(() => import("./IntermediateComponent.js"));
```

### 45. 为什么React使用className来替代class属性？
class是Javascript关键词，而且JSX是Javascript的一个扩展。这是为什么React使用className替代class的根本原。像一个prop一样传递className
```
render() {
  return <span className={'menu navigation-menu'}>{'Menu'}</span>
}
```

### 46. 什么是React的Fragments？

这是React中组件返回多个Elements的常用模式。Fragments让你可以直接返回一串子组件，不需要添加额外的DOM。

```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  )
}
```
这是一个短版，但是有很多工具中不支持这样
```
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  )
}
```
### 47. 为什么fragment比container DOM更好？

- Fragments的速度和内存占用上都比额外创建一个DOM要更快更小一点。但是这个又是只有在一个很深很大的DOM树上，才能真正体现出他的优势。
- 一些css的机制类似flex和Grid布局，有一个非常特殊的父子关系，如果添加一个中间的div的话，会让他们的布局变得麻烦
- DOM看起来不那么杂乱

### 48. 什么是React中Portals？

React Portal 是一种优秀的方法，可以将子组件渲染到由组件树层次结构定义的父 DOM 层次结构之外的 DOM 节点中。Portal 的最常见用例是子组件需要从视觉上脱离父容器的情况。
可以使用 ReactDOM.createPortal(child, container)创建一个 Portal。这里的 child 是一个 React 元素、片段或字符串，而 container 是 Portal 应该注入到的 DOM 位置（节点）。

```
const Modal =({ message, isOpen, onClose, children })=> {
  if (!isOpen) return null
  return ReactDOM.createPortal(    
    <div className="modal">
      <span className="message">{message}</span>
      <button onClick={onClose}>Close</button>
    </div>,
    domNode)
}
```
*我们为什么需要他？*

当我们在特定元素（父组件）中使用模态时，模态的高度和宽度将从模态所在的组件继承。因此，模态可能会被裁剪，而无法在应用程序中正确显示。传统上模态需要 CSS 属性，如 overflow:hidden 和 z-index，以避免出现这一问题。

### 49. 什么是无状态组件?

如果行为完全独立于自己的状态，可以被称作一个无状态组件。你可以使用function或者ES6 Class的方式来创建无状态组件。除非你需要使用到生命周期中的勾子函数，其他情况推荐使用function来创建组件。使用function来创建无状态组件有以下好处：

- 非让容易去coding，理解和测试
- 速度较快
- 可以避免使用`this`关键字

### 50. 什么是有状态组件？

如果一个组件行为依赖一个组件的状态，那么它被称作一个状态组件。这是组件通常被ES6的Class来实现，并且组件的状态在`constructor()`中被初始化。

```
class App extends Component {
  constructor(props) {
    super(props)
    this.state = { count: 0 }
  }

  render() {
    // ...
  }
}
```

React 16.8的更新后，你可以使用hooks。hooks使用你可以初始化state无需使用classes的方法
这和上面的方法是一样的
```
 import React, {useState} from 'react';

 const App = (props) => {
   const [count, setCount] = useState(0);

   return (
     // JSX
   )
 }
```

### 51. 如何对props做类型校验？

当应用部署在开发环境的时候，React会自动检查所有我们设置在组件上的props来确保他的格式正确。如果格式不正确的话，React会返回warning的信息在console当中。 为了能够有更好的表现他在生产环境中被禁止，你可以使用`isRequired`来强制检查。

预定义的prop 类型如下：

- `PropTypes.number`
- `PropTypes.string`
- `PropTypes.array`
- `PropTypes.object`
- `PropTypes.func`
- `PropTypes.node`
- `PropTypes.element`
- `PropTypes.bool`
- `PropTypes.any`

我们可以用以下方式来创建`User`组件：
```
import React from 'react'
import PropTypes from 'prop-types'

class User extends React.Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
  }

  render() {
    return (
      <>
        <h1>{`Welcome, ${this.props.name}`}</h1>
        <h2>{`Age, ${this.props.age}`}</h2>
      </>
    )
  }
}
```
注意：在React V15.5中，他被移动到了prop-type库中

等价的Function创建组件

```
import React from 'react'
import PropTypes from 'prop-types'

function User({name, age}) {
  return (
    <>
      <h1>{`Welcome, ${name}`}</h1>
      <h2>{`Age, ${age}`}</h2>
    </>
  )
}

User.propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
  }

```

### 52. React的优势？

React的主要优势如下：

- 使用虚拟DOM来提升应用的表现
- JSX让代码易读易写
- 他在服务端和客户端同时渲染
- 易于和其他框架整合
- 容易去书写单元测试

### 53. React的局限性有哪些

这些是React的主要局限性：

- 对web新手来说学习曲线较高
- React只是一个库，而不会一个完整的框架
- React兼容传统的MVC需要一系列的配置
- 太多的小组件会导致框架被过度设计
- 内联代码和JSX会增加代码的复杂性

### 54. 什么是React 16中的边界错误？

错误边界是组件在子组件树的任何位置捕获，记录这些错误，同时展示后备UI而不是崩溃组件树的组件。
如果一个组件在生命周期里定义了`componentDidCatch(error, info)`或者`static getDerivedStateFromError()`方法， 那么他就会称为错误边界。

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }

  componentDidCatch(error, info) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info)
  }

  static getDerivedStateFromError(error) {
     // Update state so the next render will show the fallback UI.
     return { hasError: true };
   }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>{'Something went wrong.'}</h1>
    }
    return this.props.children
  }
}
```
之后像使用一个常用的组件一样使用
```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

### 55. 在React 15中如何处理错误边界问题？

React 15使用`unstable_handleError`提供了非常基础的错误边界支持。他在React 16中被更名为`componentDidCatch`。

### 56. 有没有什么推荐的静态类型检查方法？

通常我们使用`PropTypes`来做React应用的类型检查。在较大的项目中，我们推荐使用FLow或者Typescript来进行类型检查。

### 57. React-dom包的作用

`React-DOM`包提供了特定的DOM方法，可以被应用在你app的顶层。大多数组件不需要使用该模块。有一些方法存在在这个包中：
- `render()`
- `hydrate()`
- `unmountComponentAtNode()`
- `findDOMNode()`
- `createPortal()`

### 58. 渲染`react-dom`的目的

这个方法用于在提供的container的DOM中插入React元素，同时返回这个组件。如果这个组件之前在container中渲染过，他将触发更新，并且仅根据需要更改 DOM 以反映最新更改。
`ReactDOM.render(element, container[, callback])`
callback非必选项，如果调用方提供了callback选项，那么他将会在更新后被执行。

### 59. 什么React DOMServer？
`ReactDOMServer`对象允许在渲染时对组件进行惊天标记（通常使用Node Server）对象主要被用在SSR这种。下面的方法可以在服务器环境和浏览器环境中都适用：
- renderToString()
- renderToStaticMarkup()
举例来说，你启动了一个基于Node的服务，像Express，Hapi或者koa等，你可以调用`renderToString()`来以字符串格式渲染你根部组件，然后将其作为响应发送

### 60. 如何在React中使用innerHTML？

`dangerouslySetInnerHTML`属性是React用来替代使用`innerHTML`在浏览器中使用DOM。就像`innerHTML`, 考虑到XSS攻击问题，使用此属性是有风险的。你只需要传递一个对象`__html`作为key而HTML文本作为value。

这个例子是`MyComponent`使用`dangerousSetInnerHTML`设置HTML标记
```
function createMarkup() {
  return { __html: 'First &middot; Second' }
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />
}
```

### 61. 如何在React中使用样式

`style`属性接受Javascript对象，属性名使用驼峰属性命名而不是css字符串。他延续了DOM的样式风格和Javscript的属性， 更高效的同时，也组织了XSS注入的安全问题。
```
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')'
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>
}
```
`style` key使用驼峰的命名方式，是为了延续Javascirpt访问DOM节点属性的方式（e.g.: node.style.backgroundImage)

### 62. 事件在React上有什么不同？

React的事件处理上有一些句法的区别：

- React的事件处理使用驼峰命名
- JSX中你传递一个方法到事件处理，而不是一个字符串

### 63. 如果你在`constructor`中使用`setState()`会发生什么？

当时你使用`setState()`，除了分配状态给对象外，他还会重新渲染组件及其所有子组件。你还会看到错误报警，像你只可以在已经挂载或者正在挂载的组件中更新状态。所以我们需要在`constructor()`中使用`this.state`来初始化变量。

### 64. 用index当key的影响是什么？

Key应该是稳定的，可预测和唯一的，所以React可以追踪所有的元素。 在下面这段代码片段中，元素将根据排序作为key，而不是与所表现的数据关联。这限制了React的性能优化。
```
{todos.map((todo, index) =>
  <Todo
    {...todo}
    key={index}
  />
)}
```
如果你用元素的数据作为唯一key，假设这个id在list当中唯一且稳定。React可以不用重新评估他们就可以排序这个元素。

```
{todos.map((todo) =>
  <Todo {...todo}
    key={todo.id} />
)}
```

### 65. 在`componentWillMount()`中使用`setState()`好吗？

是的，在`componentWillMount()`中使用`setState()`是安全的。但是，同时我们建议避免在生命周期勾子`componentWillMount()`中异步初始化。`componentWillMount()`会在挂载行为发生时立刻调用。他的调用时机早于`render()`，所以设置状态的方法不会出发重新渲染。 避免在此方法中引用任何副作用或者订阅。我们需要确定所有的异步初始化发生在`componentDidMount()`而不是`componentWillMount()`

```
componentDidMount() {
  axios.get(`api/todos`)
    .then((result) => {
      this.setState({
        messages: [...result.data]
      })
    })
}
```

### 66. 如果你在在初始化状态的时候使用props会发生什么？

如果在组件乜有发生刷新的情况下props发生了改变，新的prop内容不会被展现，因为constructor方法不会更新组件当前的状态。props初始化的状态只有在组件创建的时候才会被执行。

下面的组件不会展示更新的输入状态
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      records: [],
      inputValue: this.props.inputValue
    };
  }

  render() {
    return <div>{this.state.inputValue}</div>
  }
}

```
直接在方法当中应用则会更新数值
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      record: []
    }
  }

  render() {
    return <div>{this.props.inputValue}</div>
  }
}
```

### 67. 如何有条件的渲染组件？

在有些情况下，你想在不同情况加不同组件。JSX不渲染false或者undefine的情况，所以你可以使用简短的条件限定，只在给出条件为真的情况下渲染

```
const MyComponent = ({ name, address }) => (
  <div>
    <h2>{name}</h2>
    {address &&
      <p>{address}</p>
    }
  </div>
)
```
如果你想使用`if-else`，你可以使用三元表达式
```
const MyComponent = ({ name, address }) => (
  <div>
    <h2>{name}</h2>
    {address
      ? <p>{address}</p>
      : <p>{'Address is not available'}</p>
    }
  </div>
)
```

### 68. 为什么注意直接在DOM上进行props传递？

当我们传递props的时候，会经历向HTML上添加未知的属性的风险，这是一个错误的做法。我们使用`...rest`操作来对prop进行解构，这样他只会添加需要的props。
举例来说：
```
const ComponentA = () =>
  <ComponentB isDisplay={true} className={'componentStyle'} />

const ComponentB = ({ isDisplay, ...domProps }) =>
  <div {...domProps}>{'ComponentB'}</div>
```

### 69. 如何在React中使用装饰符？

你可以装饰你的class组件，这和像一个function中传递一个组件是一样的。装饰符是修改组件功能的灵活切易读的方式。
```
@setTitle('Profile')
class Profile extends React.Component {
    //....
}

/*
  title is a string that will be set as a document title
  WrappedComponent is what our decorator will receive when
  put directly above a component class as seen in the example above
*/
const setTitle = (title) => (WrappedComponent) => {
  return class extends React.Component {
    componentDidMount() {
      document.title = title
    }

    render() {
      return <WrappedComponent {...this.props} />
    }
  }
}
```

### 70. 如何memoization一个组件

有一些memoization化的库可以应用在function components上

举例来说`moize`库可以memoization化一个组件在其他组件中

```
import moize from 'moize'
import Component from './components/Component' // this module exports a non-memoized component

const MemoizedFoo = moize.react(Component)

const Consumer = () => {
  <div>
    {'I will memoize the following entry:'}
    <MemoizedFoo/>
  </div>
}
```
注意：从React v16.6开始，我们拥有了一个API `React.memo`。它提供一个高阶组件可以memoization组件，除非props发生改变。使用这个方法，只需要简单的在component外层用`React.memo`包裹就可以
```
 const MemoComponent = React.memo(function MemoComponent(props) {
    /* render using props */
  });
  OR
  export default React.memo(MyFunctionComponent);
```

### 71. 你如何进行服务端你渲染？

React已经准备好了处理NodeJS的服务端渲染。一个特殊的DOM渲染版本已经可用，他的模式和客户端渲染是相同的。
```
import ReactDOMServer from 'react-dom/server'
import App from './App'

ReactDOMServer.renderToString(<App />)
```
这个方法会像字符串以严格输出传荣的HTML。他可以作为一个服务器响应放在一个页面的body中。在客户端上，React会检测到预渲染的内容，并无缝的从终端的地方继续渲染。

### 72. React如何使用production（生产）环境？

您应该使用 Webpack 的 DefinePlugin 方法将 NODE_ENV 设置为生产环境，从而去除诸如 propType 验证和额外警告之类的内容。 除此之外，如果你缩小代码，例如，Uglify 的死代码消除以去除仅开发代码和注释，它将大大减少你的包的大小。

### 73. CRA是什么和他的优势？

`create-react-app` CLI工具可以帮你无配置过程的创建和运行React项目
我们来使用CRA创建一个TODO App
```
# Installation
$ npm install -g create-react-app

# Create new project
$ create-react-app todo-app
$ cd todo-app

# Build, test and run
$ npm run build
$ npm run test
$ npm start
```
这里面包含了你来创建一个React App的所有事情
- React，JSX, ES6和FLOW的语法支持
- ES6之外的语言扩展，如对象扩展运算符
- 自适应的CSS
- 一个快速的交互式单元测试运行器，内置对覆盖率报告的支持。
- 一个实时的开发环境错误报警服务器
- 一个基于hash和sourcemap的JS,CSS，图片生产环境打包脚本

### 74. 生命周期mounting安装的方法顺序是什么？

当一个组件实例正在被创建和插入到DOM当中时候，生命周期的防范按照下面的顺序进行调用
- `constructor()`
- `static getDerivedStateFromProps()`
- `render()`
- `componentDidMount()`

### 75. React v16中将要移除的生命周期方法有哪些

下面生命周期方法可能会造成不安全的代码使用并制造了很多异步渲染的问题

- `componentWillMount()`
- `componentWillReceiveProps()`
- `componentWillUpdate()`

### ***76. 生命周期方法`getDerivedStateFromProps()`创建的目的是什么？***

新的静态生命周期方法`getDerivedStateFromProps()`会在组件实例化也就是组件还没有被重新渲染的时候执行。他会返回一个对象来更新状态，或者说返回`null`声明新的props不需要任何的更新。

```
class MyComponent extends React.Component {
  static getDerivedStateFromProps(props, state) {
    // ...
  }
}
```

### ***77. 生命周期方法`getSnapshotBeforeUpdate()`创建的目的是什么？*** ###

这个方法`getSnapshotBeforeUpdate()`在DOM更新之前执行。这个方法的返回值会被当做第三个参数传递给方法`componentDidUpdate()`
```
class MyComponent extends React.Component {
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // ...
  }
}
```
生命周期中`componentDidUpdate()`覆盖了方法`componentWillUpdate()`的所用用例。

### ***78. Hooks取代了render props和高阶组件嘛？*** ### 

Render Props和高阶组件都只渲染一个子节点，但是大多数情况下Hooks是一个提供减少你树嵌套服务的更简单的方式。

### ***79. 组件推荐的命名方式*** ###

推荐使用索引而不是displayName来命名组件
用`displayName`来命名组件
```
export default React.createClass({
  displayName: 'TodoApp',
  // ...
})
```
推荐的方式：
```
export default class TodoApp extends React.Component {
  // ...
}
```

### ***80. 在Component中推荐的方法顺序是什么样的*** ###

- `static`方法
- `constructor()`
- `getChildContext()`
- `componentWillMount()`
- `componentDidMount()`
- `componentWillReceiveProps()`
- `shouldComponentUpdate()`
- `componentWillUpdate()`
- `componentDidUpdate()`
- `componentWillUnmount()`
- 事件处理方法
- 内容获取方法像`getSelectReason()`
- 条件渲染方法`renderNavigation()`或者`renderProfilePicture()`

### ***81. 什么是Switching Component*** ###

Switching Component是指从多个组件中渲染其中的一个，我们需要维护一个map来管理这些组件的props

举例来说，一个switching component基于page属性展现不同的页面

```
import HomePage from './HomePage'
import AboutPage from './AboutPage'
import ServicesPage from './ServicesPage'
import ContactPage from './ContactPage'

const PAGES = {
  home: HomePage,
  about: AboutPage,
  services: ServicesPage,
  contact: ContactPage
}

const Page = (props) => {
  const Handler = PAGES[props.page] || ContactPage

  return <Handler {...props} />
}

// The keys of the PAGES object can be used in the prop types to catch dev-time errors.
Page.propTypes = {
  page: PropTypes.oneOf(Object.keys(PAGES)).isRequired
}
```

### ***82. 我们为什么需要像setState()中传递一个方法*** ###

这个原因是，`setState()`是一个异步方法。React中批量state更新时，为了渲染表现，组件的状态有时候并不是`setState()`调用后立刻发生改变的。这意味着你不能在调用`setState()`就依赖state现在的数据，因为你不知道什么时候这个数据会更新。解决方法就是传递一个方法到`setState()`中，把之前的状态当做一个参数。这样可以避免你在使用state方法中，获取到旧的未更新数据。

我们来看一个例子，初始化的count是0，经过连续3次的增加操作，每次只增加1.
```
// assuming this.state.count === 0
this.setState({ count: this.state.count + 1 })
this.setState({ count: this.state.count + 1 })
this.setState({ count: this.state.count + 1 })
// this.state.count === 0, not 3
```
如果我们是像`setState()`中传入一个方法，那么counts则会增长正确。

```
this.setState((preState, props) => {
  count: prevState.counter + props.increment
})
```

### ***83. 什么是React中的严格模式(Strict Mode)*** ###

`React.StrictMode`是一个用来在应用中，帮我们发现潜在错误的组件。就像`<Fragment>`, `<StrictMode>`不会渲染额外的DOM。他会对当前和当前的子孙组件进行检查。而且该检查模式只在`development`模式下生效。

```
import React from 'react'

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Header />
    </div>
  )
}
```
在上面这个例子中，严格模式(strict mode)只检查`ComponentOne`和`ComponentTwo`两个组价

### ***84. 什么是React Mixins*** ###

Mixins已经废弃（引入一个公共组件，抽象其生命周期的方法达到抽象公共方法提供不同模块使用的目的），现在可以用hooks或者高阶组件替代。

### ***85. 为什么`isMounted()`是反模式，什么是合理的解决方法？*** ###

`isMounted()`创建最初的想法是当一个组件不再挂载的时候避免继续调用`setState()`，因为这样的操作会发出一个warning

```
if (this.isMounted()) {
  this.setState({...})
}
```
在`setState()`之前检查`isMounted()`是可以避免warning的情况，但这违背的警告的目的。使用`isMounted()`是一种代码异味，因为你使用这个方法的唯一原因就是你认为组件在解除挂在后仍然在被引用。

一个有效的解决方案是，检查哪个地方在组件解除挂载后仍在持续调用`setState()`。这种情况出现通常出现在回调当中，当一个组件在等待数据返回的时候，在数据返回之前，组件就已经解除挂载。理想状态下，所有的回调应该在`componentWillUnmount()`中，优先于`unmounting`。

### ***86. React支持哪些Pointer事件？*** ###

Pointer 事件提供了一个统一的方式来处理这些输入事件。在过去我们有鼠标事件，各自的事件回调去处理各自的方法。今天，我们有很多设备没有鼠标，像手机或者pad使用点击或者笔的触屏方式。我们需要记住这些事件只在浏览器中有效。

下面的事件现在在React DOM中有效
- `onPointerDown`
- `onPointerMove`
- `onPointerUp`
- `onPointerCancel`
- `onGotPointerCapture`
- `onLostPointerCapture`
- `onPointerEnter`
- `onPointerLeave`
- `onPointerOver`
- `onPointerOut`

### ***87. 为什么组件名的首字母要大写？*** ###

如果你在用JSX生成你的组件，那么你的组件首字母需要用大写字母。否则React会抛出一个错误。这个惯例应用的原因是，只有HTML和svg标签的首字母是小写。
```
class SomeComponent extends Component {
 // Code goes here
}
```
你可以定义一个首字母为小写的的组件，但是在引用的时候，他的首字母需要是大写的。这里小写是可以的
```
class myComponent extends Component {
  render() {
    return <div />
  }
}
export default myComponent
```
当引用的时候，他的首字母需要是大写
```
import MyComponent from './MyComponent'
```

***React组件命名有哪些例外？***
##### 组件的命名首字母应该是大写，但是有一些例外，带`.`的小写标签名也被认为是有效的。举例来说，下面的例子可以被打包成一个有效的组件

```
  render() {
      return (
        <obj.component/> // `React.createElement(obj.component)`
      )
}
```

### ***88. 自定义的标签属性在React V16中还支持嘛？*** ###
是的，在过去，React会忽略未知的DOM属性标签，如果你使用JSX添加了一个属性但是React没有识别，React会直接跳过这个属性，举例来说：
```
<div mycustomattribute={'something'} />
```
这在React v15中会渲染一个空的标签
```
<div />
```
在React v16中，未知标签将会呈现一下结果：
```
<div mycustomattribute='something' />
```
这对浏览器的非常规标签非常有帮助，尝试新的DOM API, 以及集成第三方的库非常有帮助

### ***89. `constructor()` 和`getInitialState()`的区别是什么*** ###

在你使用ES6 classes，`getInitialState()`方法和`React.createClass()`的时候，你需要在constructor中初始化状态。

使用ES6 Classes：
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.state = { /* initial state */ }
  }
}
```
使用`React.createClass()`
```
const MyComponent = React.createClass({
  getInitialState() {
    return { /* initial state */ }
  }
})
```
注意：CreateClass在React 16中已经被移除。

### ***90.可以不调动`setState()`的情况下强制一个组件刷新嘛？*** ###

一般情况下React的组件会在state或者props发生变的话的时候重新渲染，如果你的`render()`方法依赖其他数据，你可以使用`forceUpdate()`告诉React什么时候重新渲染组件。
```
component.forceUpdate(callback)
```
但是不建议使用`forceUpdate()`, 尽量将组件的数据都维护在state和props中

### ***91. ES6中，super()和super(props)的区别是什么？*** ###

`super()`直接调用不能在`constructor()`中使用`this.props`，但是使用`super(props)`可以。

### ***92. JSX中如何循环*** ###

你可以直接使用ES6的箭头函数，使用`Array.prototype.map`
举例来说：
```
<tbody>
  {items.map(item => <SomeComponent key={item.id} name={item.name} />)}
</tbody>
```
但是你不能使用for来循环：
```
<tbody>
  for (let i = 0; i < items.length; i++) {
    <SomeComponent key={items[i].id} name={items[i].name} />
  }
</tbody>
```
这是因为 JSX 标签被转译为函数调用，并且您不能在表达式中使用语句。由于第 1 阶段提案的 do 表达式，这可能会改变。

### ***93.你如何在属性中访问props*** ###

React和JSX不支持属性插值，下面这样的代码不会生效
```HTML
<img className='image' src='images/{this.props.image}' />
```
但是您可以将任何 JS 表达式放在大括号内作为整个属性值。 所以下面的表达式有效：
```HTML	
<img className='image' src={'images/' + this.props.image} />
```
用模板语句也同样有效:
```HTML
<img className='image' src={`images/${this.props.image}`} />
```

### ***94.React的数组的prototype的shape*** ###

如果你想向一个组件中传递一个特殊shape的数组则使用`React.ProtoTypes.shape()`当做一个参数传入到`React.ProtoTypes.arrayOf()`
```JavaScript	
ReactComponent.propTypes = {
  arrayWithShape: React.PropTypes.arrayOf(React.PropTypes.shape({
    color: React.PropTypes.string.isRequired,
    fontSize: React.PropTypes.number.isRequired
  })).isRequired
}
```
### ***95. 如何根据条件添加DOM属性*** ###

你不应该用引号讲其包裹起来，因为这样它会被当做一个字符串处理

```HTML	
<div className="btn-panel {this.props.visible ? 'show' : 'hidden'}">
```

你应该将大括号移动到最外面
```HTML
<div className={'btn-panel ' + (this.props.visible ? 'show' : 'hidden')}>
```
模板语句同样生效
```HTML
<div className={`btn-panel ${this.props.visible ? 'show' : 'hidden'}`}>
```

### ***96. React和React DOM的区别*** ###
`React`的包中，包含了`React.createElements()`, `React.Component`, `React.children`和其他与元素或组件相关的类。你可以认为这些是同构或者创建组件的万能帮手。而在`React-dom`中，其包含`ReactDOM.render()`, 而且在`react-dom/server`中，我们有Server side render(SSR)来支持`ReactDOMServer.renderToString()`和`ReactDOMServer.renderToStaticMarkup()`。

### ***97.为什么React DOM从React中分离出来？*** ###

这样React做的事情与DOM无关，为React.Native等合作铺路。

### ***98. 如何使用React的标签元素*** ###

如果您尝试使用标准 for 属性呈现绑定到文本输入的 `<label>` 元素，则会生成缺少该属性的 HTML 并向控制台打印警告。

```html
<label for={'user'}>{'User'}</label>
<input type={'text'} id={'user'} />
```
因为`for`是一个默认字段，这里我们用htmlFor来替代
```html
<label htmlFor={'user'}>{'User'}</label>
<input type={'text'} id={'user'} />
```

### ***99. 如何在一行里合并样式对象*** ###

你可以在React中使用使用扩展运算符
```html
 <button style={{...styles.panel.button, ...styles.panel.submitButton}}>{'Submit'}</button>
```

### ***100. 如何在窗口大小发生改变的时候重新渲染组件*** ### 
在`componentDidMounted()`中监听resize事件，并移除`componentWillUpadted()`方法
```javascript
class WindowDimensions extends React.Component {
  constructor(props){
    super(props);
    this.updateDimensions = this.updateDimensions.bind(this);
  }
   
  componentWillMount() {
    this.updateDimensions()
  }

  componentDidMount() {
    window.addEventListener('resize', this.updateDimensions)
  }

  componentWillUnmount() {
    window.removeEventListener('resize', this.updateDimensions)
  }

  updateDimensions() {
    this.setState({width: window.innerWidth, height: window.innerHeight})
  }

  render() {
    return <span>{this.state.width} x {this.state.height}</span>
  }
}
```
