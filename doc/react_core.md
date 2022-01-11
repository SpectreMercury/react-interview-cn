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