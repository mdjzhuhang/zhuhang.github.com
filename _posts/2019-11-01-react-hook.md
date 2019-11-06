---
layout: post
title: React Hook 入门
excerpt: react组件：类（class）写法复杂，function写法不能包含状态，也不支持生命周期方法，React Hooks 的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件。
category: frontend
---

v16.8 版本之前，组件的标准写法是类（class），真实的 React App 由多个类按照层级，一层层构成，复杂度成倍增长。再加入 Redux，就变得更复杂。

Redux 的作者 Dan Abramov 总结了组件类的几个缺点。

    大型组件很难拆分和重构，也很难测试。
    业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
    组件类引入了复杂的编程模式，比如 render props 和高阶组件。


所以，组件的最佳写法应该是函数，而不是类。

React 早就支持函数组件，例：

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
但是，这种写法必须是纯函数，不能包含状态，也不支持生命周期方法，因此无法取代类。

React Hooks 的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件。

#### Hook 的含义

组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。 React Hooks 就是那些钩子。

你需要什么功能，就使用什么钩子。React 默认提供了一些常用钩子，你也可以封装自己的钩子。

所有的钩子都是为函数引入外部功能，所以 React 约定，钩子一律使用use前缀命名，便于识别。你要使用 xxx 功能，钩子就命名为 usexxx。

下面介绍 React 默认提供的四个最常用的钩子。
```
useState()
useContext()
useReducer()
useEffect()
```

#### useState()：状态钩子
Ps: 多个useState时，没有key，react是根据 useState 出现的顺序来定的，所以 useState 不要写在 if else 等条件语句当中


button 组件 class写法：
```
import React, { Component } from "react";

export default class Button extends Component {
  constructor() {
    super();
    this.state = { buttonText: "Click me, please" };
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState(() => {
      return { buttonText: "Thanks, been clicked!" };
    });
  }
  render() {
    const { buttonText } = this.state;
    return <button onClick={this.handleClick}>{buttonText}</button>;
  }
}
```
react hook 写法：
```
import React, { useState } from "react";

export default function  Button()  {


  const  [buttonText, setButtonText] =  useState("Click me,   please");
  // [ 1变量:状态的当前值，2函数:用来更新状态 ]
  // 约定函数名为：set前缀加上状态的变量名
  
  
  function handleClick()  {
    return setButtonText("Thanks, been clicked!");
  }

  return  <button  onClick={handleClick}>{buttonText}</button>;
}

```

#### useContext()：共享状态钩子

```
import React, { useContext } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

// 在组件外部建立一个 Context

const AppContext = React.createContext({});

// 封装组件:

function App() {
  // AppContext.Provider提供了一个 Context 对象，这个对象可以被子组件共享。
  return (
    <AppContext.Provider value={{
      username: 'superawesome'
    }}>
      <div className="App">
        <Navbar />
        <Messages />
      </div>
    </AppContext.Provider>
  );
}

// Navbar 组件:

const Navbar = () => {
  const { username } = useContext(AppContext);
  return (
    <div className="navbar">
      <p>AwesomeSite</p>
      <p>{username}</p>
    </div>
  );
}

// Message 组件

const Messages = () => {
  const { username } = useContext(AppContext)

  return (
    <div className="messages">
      <h1>Messages</h1>
      <p>1 message for {username}</p>
      <p className="message">useContext is awesome!</p>
    </div>
  )
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

```

#### useReducer()：action 钩子

状态管理最常用外部库--Redux，核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态，
```
(state, action) => newState
```

useReducers() 钩子用来引入 Reducer 功能。

```
const [state, dispatch] = useReducer(reducer, initialState);
```
ps:  Hooks 可以提供共享状态和 Reducer 函数，所以它在这些方面可以取代 Redux。但是不提供Redux的中间件（middleware）和时间旅行（time travel）功能

```
import React, { useReducer } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

const myReducer = (state, action) => {
  switch(action.type) {
    case('countUp'):
      return {
        ...state,
        count: state.count + 1
      }
    default:
      return state
  }
}

function App() {
  const [state, dispatch] = useReducer(myReducer, { count: 0 })
  // [状态的当前值，发送action的dispatch函数] = useReducer(Reducer函数, 状态的初始值);
  return (
    <div className="App">
      <button onClick={() => dispatch({ type: 'countUp' })}>
        +1
      </button>
      <p>Count: {state.count}</p>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
#### useEffect()：副作用（side effect）钩子
useEffect()用来引入具有副作用的操作，最常见的就是向服务器请求数据。以前，放在 componentDidMount，componentDidUpdate 或 componentWillUnmount 里面，现在可以放在useEffect()

```
useEffect(()  =>  {
  // Async Action
}, [dependencies])
```

只要 dependencies 数组(异步函数的依赖)发生变化，useEffect()就会执行。

组件第一次渲染时，useEffect()也会执行

省略 dependencies 时每次组件渲染时，都会执行。

dependencies 为 []，只在组件第一次渲染时执行。

想要同步执行。例如想要等计算DOM尺寸再重新渲染，就不能用useEffect()代替componentDidMount或componentDidUpdate.

每当 componentDidMount 里添加了一个注册，得马上在componentWillUnmount中，也就是组件被注销之前清除掉我们添加的注册，否则内存泄漏。而每次组件渲染时，都执行 useEffect() 的话，就不用专门解绑


```
const Person = ({ personId }) => {
  const [loading, setLoading] = useState(true);
  const [person, setPerson] = useState({});

  useEffect(() => {
    setLoading(true); 
    fetch(`https://swapi.co/api/people/${personId}/`)
      .then(response => response.json())
      .then(data => {
        setPerson(data);
        setLoading(false);
      });
  }, [personId])

  if (loading === true) {
    return <p>Loading ...</p>
  }

  return <div>
    <p>You're viewing: {person.name}</p>
    <p>Height: {person.height}</p>
    <p>Mass: {person.mass}</p>
  </div>
}
```

#### 创建自己的 Hooks

自定义 usePerson:
```
const usePerson = (personId) => {
  const [loading, setLoading] = useState(true);
  const [person, setPerson] = useState({});
  useEffect(() => {
    setLoading(true);
    fetch(`https://swapi.co/api/people/${personId}/`)
      .then(response => response.json())
      .then(data => {
        setPerson(data);
        setLoading(false);
      });
  }, [personId]);  
  return [loading, person];
};

const Person = ({ personId }) => {
  const [loading, person] = usePerson(personId);

  if (loading === true) {
    return <p>Loading ...</p>;
  }

  return (
    <div>
      <p>You're viewing: {person.name}</p>
      <p>Height: {person.height}</p>
      <p>Mass: {person.mass}</p>
    </div>
  );
};

```
