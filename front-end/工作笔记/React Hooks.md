# React Hooks

[阮一峰]: http://www.ruanyifeng.com/blog/2019/09/react-hooks.html
[React-Hook API]: https://zh-hans.reactjs.org/docs/hooks-reference.html

Hook 这个单词的意思是"钩子"。

**React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。** React Hooks 就是那些钩子。

钩子一律使用`use`前缀命名，便于识别。你要使用 xxx 功能，钩子就命名为 usexxx。

React 基础Hook

- [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate)
- [`useEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useeffect)
- [`useContext`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext)

额外的Hook

- [`useReducer`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usereducer)
- [`useCallback`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)
- [`useMemo`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usememo)
- [`useRef`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useref)
- [`useImperativeHandle`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)
- [`useLayoutEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect)
- [`useDebugValue`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usedebugvalue)



### 一、useState()：状态钩子

******

**作用：**用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。

**语法：**`useState(状态的初始值)`——该函数返回数组 `[状态变量名, 函数]`

- **状态变量名：**指向状态的当前值,

- **函数：**更新状态；约定命名规则‘set变量名首字母大写’

```

const  [buttonText, setButtonText] =  useState("Click me,   please");
```





### 二、useContext()：共享状态钩子

************

**作用：**组件之间共享状态

**语法：**`useContext(AppContext)` ——返回`AppContext.Provider`提供的`Context 对象`（共享状态）

**案例：**现在有两个组件 Navbar 和 Messages，我们希望它们之间共享状态。

```
import React, { useContext } from "react";
import ReactDOM from "react-dom";
import "./styles.css";
// 1、创建：使用 React Context API，在组件外部建立一个 Context。===============
const AppContext = React.createContext({});

// 3、使用：上面代码中，`AppContext.Provider`提供了一个 Context 对象，这个对象可以被子组件共享。
const Navbar = () => {
  const { username } = useContext(AppContext)

  return (
    <div className="navbar">
      <p>AwesomeSite</p>
      <p>{username}</p>
    </div>
  )
}

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
// 2、封装：组件封装代码如下。=================
function App() {
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

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```



### 三、useReducer()：action 钩子

***********

**作用：**状态管理 ？

> React 本身不提供状态管理功能，通常需要使用外部库。这方面最常用的库是 Redux。
>
> Redux 的核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态，Reducer 函数的形式是`(state, action) => newState`。
>
> `useReducers()`钩子用来引入 Reducer 功能。

**语法：**

`useReducer(Reducer 函数, 状态的初始值)` ——返回一个数组 `[状态的当前值， 发送 action 的dispatch函数]`

```
const [state, dispatch] = useReducer(reducer, initialState);
```

**案例：**计数器

```
import React, { useReducer } from "react";
import ReactDOM from "react-dom";
import "./styles.css";
// 计算状态的 Reducer 函数==============
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
// 组件代码==============================
function App() {
  const [state, dispatch] = useReducer(myReducer, { count: 0 })

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



### 四、useEffect()：副作用钩子

****************

**作用：**

引入具有副作用的操作

以前，放在`componentDidMount`里面的代码，现在可以放在`useEffect()`。

> 补充知识：副作用？纯函数

**语法：**

```
useEffect(()  =>  {
  // Async Action
}, [dependencies])
```

| 参数              | 内容                                                         |        |
| ----------------- | ------------------------------------------------------------ | ------ |
| 函数              | 异步操作代码放在该函数中                                     | 非必填 |
| Effect 的依赖数组 | 有：只要这个数组发生变化，`useEffect()`就会执行，组件第一次渲染时，`useEffect()`也会执行；无：初次DOM加载后会执行一次`useEffect()`。 | 非必填 |

**案例：**

向服务器请求数据



### 五、useRef() ：跨渲染周期存储数据

*******

[React-Hook API：useRef()]: https://zh-hans.reactjs.org/docs/hooks-reference.html#usere
[帮助理解：useRef()]: https://blog.csdn.net/hjc256/article/details/102587037
**作用：**

在一个组件中有什么东西可以跨渲染周期，也就是在组件被多次渲染之后依旧不变的属性？第一个想到的应该是`state`。没错，一个组件的`state`可以在多次渲染之后依旧不变。**但是，`state`的问题在于一旦修改了它就会造成组件的重新渲染**。

那么这个时候就可以使用`useRef`来跨越渲染周期存储数据，而且对它修改也不会引起组件渲染。

**语法：**

```

```





[补充知识：JS获取DOM元素方法]: https://www.jb51.net/article/116460.htm

1. 通过ID获取（getElementById）
2. 通过name属性（getElementsByName）
3. 通过标签名（getElementsByTagName）
4. 通过类名（getElementsByClassName）
5. 获取html的方法（document.documentElement）
6. 获取body的方法（document.body）
7. 通过选择器获取一个元素（querySelector）
8. 通过选择器获取一组元素（querySelectorAll）

```
document.getElementById('id')
```



[补充知识：HTML DOM focus()]: https://www.runoob.com/jsref/met-html-focus.html

语法：

```
HTMLElementObject.focus()
```

| 参数 |
| ---- |
| None |

实例：// 为 <a> 元素设置焦点（光标在此处闪动）

```
document.getElementById("myAnchor").focus();
```



### 六、useMemo()

**********

