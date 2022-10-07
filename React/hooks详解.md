## `React Hooks`

- 状态：`useState`
- 副作用：`useEffect`
  - `useLayoutEffect`
- 上下文：`useContext`
- `Redux`：`useReducer`
- 记忆：`useMemo`
  - 回调：`useCallback`
- 引用：`useRef`
  - `uselmperativeHandle`
- 自定义：`Hook`
  - `useDebugValue`

## `setState`

1. 首次渲染组件时，`App`会被调用，得到虚拟`dom`，创建真实的`dom`
2. 当点击`button`时，调用`setN`，再次调用`App`，得到虚拟`dom`，用`DOM Diff`算法更新`dom`

```ts
function App() {
  const [n, setN] = React.useState(0);
  console.log("App 运行了"); // App 被调用就会执行一次
  console.log(`n: ${n}`); // App 被调用后 n 每次都不一样
  return (
    <div className="App">
      <p>{n}</p>
      <p><button onClick={() => setN(n + 1)}>+1</button></p>
    </div>
  );
}
```

`n`在`App`被调用后每次都会变化，但是`setN()`却不会改变`n`。

- `setN`一定会修改数据`x`，将`n + 1`存入`x`
- `setN`一定会触发`App`重新渲染`
- `useState`肯定会从`x`读取`n`的最新值
- 每个组件都有自己的数据`x`，我们将其命名成`state`

### 自己来实现`setState`

- 声明一个`myUseState`，接收一个初始值`initialValue`
- 将初始值`initialValue`赋值给一个中间变量`state`
- 内部声明一个函数`setState`，接收一个`newValue`，再将`newValue`赋值给`state`，并执行`App`
- 返回`state`和`setState`

```ts
const myUseState = initialValue => {
  let state = initialValue;
  const setState = newValue => {
    state = newValue;
    render();
  };
  return [state, setState];
};

const render = () => {
  ReactDOM.render(<App />, rootElement);
};

function App() {
  const [n, setN] = myUseState(0);
  return (
    <div className="App">
      <p>{n}</p>
      <p><button onClick={() => setN(n + 1)}>+1</button></p>
    </div>
  );
}
```

但是这样有个问题每次执行`setN`时，都会把`state`设置为初始值，因为每次执行`setN`都会传入一个初始值`0`

解决这个问题就是将`state`变成全局变量

```ts
let _state;
const myUseState = initialValue => {
  _state = _state === undefined ? initialValue : _state;
  const setState = newValue => {
    _state = newValue;
    render();
  };
  return [_state, setState];
};

const render = () => {
  ReactDOM.render(<App />, rootElement);
};

function App() {
  const [n, setN] = myUseState(0);
  return (
    <div className="App">
      <p>{n}</p>
      <p><button onClick={() => setN(n + 1)}>+1</button></p>
    </div>
  );
}
```

### 如果一个组件使用了两个`useState`

```ts
function App() {
  const [n, setN] = myUseState(0);
  const [M, setM] = myUseState(1); // 会出问题，第一个会被覆盖
  return (
    <div className="App">
      <p>{n}</p>
      <p><button onClick={() => setN(n + 1)}>+1</button></p>
      <p>{m}</p>
      <p><button onClick={() => setM(m + 1)}>+1</button></p>
    </div>
  );
}
```

由于所有数据都存在一个`_state`中，所以会冲突。

可以使用数组去解决`_state`重复问题。

- 将`_state`声明为`[]`，同时声明一个索引`index = 0`
- `myUseState`方法内部声明一个临时变量`currentIndex`，用来保存索引`index`
- 用索引去初始化`_state`
- `setState`时也将通过索引去操作`_state`
- `index += 1`
- 返回`_state[currentIndex]`和`setState`
- 每次调用`render`方法是将`index`重置为`0`

```ts
let _state = [];
let index = 0;
const myUseState = initialValue => {
  const currentIndex = index;
  _state[currentIndex] =
    _state[currentIndex] === undefined ? initialValue : _state[currentIndex];

  const setState = newValue => {
    _state[currentIndex] = newValue;
    render();
  };
  index += 1;
  return [_state[currentIndex], setState];
};

const render = () => {
  index = 0;
  ReactDOM.render(<App />, rootElement);
};

function App() {
  const [n, setN] = myUseState(0);
  const [m, setM] = myUseState(0);
  return (
    <div className="App">
      <p>{n}</p>
      <p><button onClick={() => setN(n + 1)}>N+1</button></p>
      <p>{m}</p>
      <p><button onClick={() => setM(m + 1)}>M+1</button></p>
    </div>
  );
}
```

### `useState`调用顺序

- 若第一渲染时`n`是第一个，`m`是第二个，`k`是第三个
- 则第二次渲染时必须保证顺序完全一致
- `React`不允许出现如下代码

```ts
function App() {
  const [n, setN] = myUseState(0);
  let m, setM;
  if (n % 2 === 1) [m, setM] = myUseState(0); // 报错
  return (
    <div className="App">
      <p>{n}</p>
      <p><button onClick={() => setN(n + 1)}>N+1</button></p>
      <p>{m}</p>
      <p><button onClick={() => setM(m + 1)}>M+1</button></p>
    </div>
  );
}
```

报错信息：`React has detected a change in the order of Hooks called by App. This will lead to bugs and errors if not fixed.`

### 问题：

`App`用了`_state`和`index`那其他组件用什么？

- 给每个组件创建一个`_state`和`index`

放在全局作用域重名了咋整

- 放在组件对象的虚拟节点对象上

### 感觉会是`bug`

```ts
function App() {
  const [n, setN] = myUseState(0);
  const log () => setTimeout(() => console.log(`n: ${n}`), 3000)
  return (
    <div className="App">
      <p>{n}</p>
      <p>
        <button onClick={() => setN(n + 1)}>N+1</button>
        <button onClick={log}>log</button>
      </p>
    </div>
  );
}
```

先点击`N+1`时，再点击`log`，输出是没有问题

如果先点击`log`，在点击`N+1`，就会发现输出的居然是`0`。难道`N+1`后输出不是`1`么。

因为`setN`是不会改变`n`，而是生成一个新的`n`。

### 解决上面那个感觉是`bug`

- 全局变量，`window.xxx`
- `useRef`不仅可以用于`div`，还能用于任意数据
- `useContext`不能能贯穿始终，还能贯穿不同组件

```ts
function App() {
  const nRef = React.useRef(0);   // { current: 0 }
  const log () => setTimeout(() => console.log(`n: ${React.useRef(0)}`), 3000);
  const update = React.useState(null)[1];
  return (
    <div className="App">
      <p>{nRef.current}</p>
      <p>
        <button onClick={() => {
          nRef.current += 1;  // 这里不能实时更新
          update(nRef.current;
          )}}>N+1</button>
        <button onClick={log}>log</button>
      </p>
    </div>
  );
}
```

`React.current += 1`不会让`App`重新渲染。

### 注意

- 不可以局部更新，因为`setState`不会合并属性
- 对象地址要变
- `useState`接收函数
  - `js`引擎不会去解析函数，减少多余的计算过程
- `setN`接收函数，两步操作都会生效
  ```ts
  setN(i => i + 1);
  setN(n => n + 1);
  ```

### 总结

- 每个函数组件对象一个`React`节点
- 每个节点保存着`state`和`index`
- `useState`会读取`state[index]`
- `index`由`useState`出现的顺序决定
- `setState`会修改`state`，并触发更新。

`Tips`：

1. 这里是对`React`做了简化，方便理解。
2. `React`对象节点应该是`FiberNode`
3. `_state`的真实名称为`memorizedState`；`index`的实现用到了[链表](https://juejin.im/post/5bdfc1c4e51d4539f4178e1f)

## `useReducer`

用来践行`Flux/Redux`的思想

- 创建初始值`initial`
- 创建所有操作`reducer(state, action)`
- 传给`useReducer`，得到读和写的`API`
- 调用`写({type: '操作类型'})`

总的来说`useReducer`是`useState`的复杂版

```ts
const initial = { n: 0}
const reducer = (state, action) => {
  if(action.type === "add") {
    return { n: state.n + action.number }
  } else if(action.type === "multi"){
    return { n: state.n + action.number }
  } else {
    throw new Error("unknow type")
  }
}

functon App() {
  const [state, dispatch] = React.useReducer(reducer, initial)
  const {n} = state
  const onClick = () => { dispatch({type: "add", number: 1}) }
  const onClick2 = () => { dispatch({type: "multi", number: 2}) }

  return (
    <div className="App">
      <h1>n：{n}</h1>
      <button onClick={onClick}>+1</button>
      <button onClick={onClick2}>+1</button>
    </div>
  )
}
```

### 什么时候使用

如果发现有几个变量适合放在一起就用`useReducer`，否则就用`useState`

### 使用`useReducer`代替`redux`

```ts
const store = { user: null, books: null, movies: null };
const reducer = (state, action) => {
  switch (action.type) {
    case "setUser":
      return { ...state, user: action.user };
    case "setBooks":
      return { ...state, books: action.books };
    case "setMovies":
      return { ...state, movies: action.movies };
  }
};
const Context = createContext(null);
function App() {
  const [state, dispatch] = useReducer(reducer, store);

  return (
    <Context.Provider value={{ state, dispatch }}>
      <User />
      <hr />
      <Books />
      <Movies />
    </Context.Provider>
  );
}

function User() {
  const { state, dispatch } = useContext(Context);
  useEffect(() => {
    ajax("/user").then(user => {dispatch({ type: "setUser", user });});
  }, []);
  return (
    <div>
      <h1>个人信息</h1>
      <div>name：{state.user ? state.user.name : ""}</div>
    </div>
  );
}

function Books() {
  const { state, dispatch } = useContext(Context);
  useEffect(() => {
    ajax("/books").then(books => {dispatch({ type: "setBooks", books });});
  }, []);
  return (
    <div>
      <h1>我的书籍</h1>
      <ol>{state.books ? state.books.map(book => <li key={book.id}>{book.name}</li>) : "加载中"}</ol>
    </div>
  );
}
function Movies() {
  const { state, dispatch } = useContext(Context);
  useEffect(() => {
    ajax("/movies").then(movies => {dispatch({ type: "setMovies", movies });});
  }, []);
  return (
    <div>
      <h1>我的电影</h1>
      <ol>{state.movies ? state.movies.map(movie => <li key={movie.id}>{movie.name}</li>) : "加载中"}</ol>
    </div>
  );
}

function ajax(path) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (path === "/user") {
        resolve({ id: 1, name: "Frank" });
      } else if (path === "/books") {
        resolve([
          { id: 1, name: "JavaScript 高级程序设计" },
          { id: 2, name: "JavaScript 精粹" }
        ]);
      } else if (path === "/movies") {
        resolve([
          { id: 1, name: "爱在黎明破晓前" },
          { id: 2, name: "恋恋笔记本" }
        ]);
      }
    }, 2000);
  });
}
```

## `useContext`

- 上下文
  - 全局变量的全局的上下文
  - 上下文是局部的全局变量

- 使用方法
  - 使用`C = createContext(initial)`创建上下文
  - 使用`<C.provider>`圈定作用域
  - 在作用域内使用`useContext(C)`来使用上下文

## `useEffect`

- 副作用
  - 对环境的改变即为副作用，如修改`document.title`
  - 但我们不一定非要把副作用放在`useEffect`里
  - 实际上叫做`afterRender`更好，每次`render`后运行

- 用途
  - 作为`componentDidMount`使用，`[]`作为第二个参数
  - 作为`componentDidUpdate`使用，可指定依赖
  - 作为`componentWillUnmount`使用，通过`return`
  - 以上三种用途可以同时存在

- 特点
  - 如果同时存在多个`useEffect`，会按照出现次序执行

### 模拟`componentDidMount`和`componentDidUpdate`

```ts
function App() {
  const [n, setN] = useState(0);
  const onClick = () => {
    setN(i => i + 1);
  };
  useEffect(() => {
    console.log("第一次渲染后执行");
  }, []);
  useEffect(() => {
    if (n !== 0) console.log("n 变化了执行");
  }, [n]);

  useEffect(() => {
    console.log("任何一个 state 变化了都执行");
  });

  return (
    <div>
      n:{n}
      <button onClick={onClick}>+1</button>
    </div>
  );
}
```

## 模拟`componentWillUnmount`

在`useEffect`内部加上`return`就可以模拟`componentWillUnmount`

```ts
function App() {
  const [n, setN] = useState(0);
  const onClick = () => {
    setN(i => i + 1);
  };
  useEffect(() => {
    const timerid = setInterval(() => {
      console.log(1);
    }, 1000);
    return () => {
      window.clearInterval(timerid);
    };
  }, []);

  return (
    <div>
      n:{n}
      <button onClick={onClick}>+1</button>
    </div>
  );
}
```

## `useLayoutEffect`

- 布局副作用
  - `useEffect`在浏览器渲染完成之后执行
  - `useLayoutEffect`在浏览器渲染前执行

- 特点
  - `useLayoutEffect`总是比`useEffect`先执行
  - `useLayoutEffect`里的任务最好影响了`Layout`

- 经验
  - 为了用途体验，优先使用`useEffect`（优先渲染）

```ts
function App() {
  const [value, setValue] = useState(0);
  useEffect(() => {
    document.querySelector("#x").innerText = `value:1000`;
  }, [value]);

  return (
    <div id="x" onClick={() => setValue(1)}>
      value:{value}
    </div>
  );
}
```

使用`useEffect`的一个问题：
  - 如果修改的是`DOM`元素，那么在页面初始化后页面显示的是`value：0`然后在变成`value：1000`，这中间就有一个闪烁的过程。

用`useLayoutEffect`代替`useEffect`就可以解决这个问题

看一下具体的过程：

```ts
App() => 执行 => 虚拟DOM => DOM => 改变外观 => useEffect
                               => useLayoutEffect
```

在渲染样式前，就对`DOM`修改，这样就能避免页面闪烁问题了。

从下面代码可以看出`useLayoutEffect`先于`useEffect`执行

```ts
function App() {
  const [value, setValue] = useState(0);
  useEffect(() => {
    console.log(1)      // 后打印
  }, []);

  useLayoutEffect(()=>{
    console.log(2)      // 先打印
  },[])

  return (
    <div id="x" onClick={() => setValue(0)}>
      value:{value}
    </div>
  );
}
```

从下面代码可以看出使用`useEffect`的时间比`useLayoutEffect`明显要长

```ts
function App() {
  const [n, setN] = useState(0);
  const time = useRef(null);
  const onClick = () => {
    setN(i => i + 1);
    time.current = performance.now();
  };

  useLayoutEffect(() => {
    if (time.current) console.log(performance.now() - time.current);
  });

  useEffect(() => {
    if (time.current) console.log(performance.now() - time.current);
  });
  
  return (
    <div id="x">
      n:{n}
      <button onClick={onClick}>+1</button>
    </div>
  );
}
```

## `memo`

- `React`默认有多余的`render`
- 如果`props`不变，就没有必要执行一个函数组件
- 但是，有个`bug`
  - 添加了监听函数之后，一秒破功
  - 因为`App`运行时会在执行监听函数，生成新的函数
  - 新旧函数虽然功能一样，但是地址不一样

```ts
function App() {
  const [n, setN] = React.useState(0);
  const [m, setM] = React.useState(0);
  const onClick = () => {
    setN(n + 1);
  };

  return (
    <div className="App">
      <div>
        <button onClick={onClick}>update n {n}</button>
      </div>
      <Child data={m} />
    </div>
  );
}

function Child(props) {
  console.log("child 执行了")
  return <div>child: {props.data}</div>;
}
```

当点击按钮的时候，`n`会变，这时`Child`组件，也会跟着变化，其实这里的`Child`组件是不需要变化的。

可以使用`memo`优化

```ts
function App() {
  const [n, setN] = React.useState(0);
  const [m, setM] = React.useState(0);
  const onClick = () => {
    setN(n + 1);
  };
  const onClick2 = () => {
    setM(m + 1);
  };
  return (
    <div className="App">
      <div>
        <button onClick={onClick}>update n {n}</button>
        <button onClick={onClick2}>update m {m}</button>
      </div>
      <Child data={m} />
    </div>
  );
}

const Child = React.memo(props => {
  console.log("child 执行了");
  return <div>child: {props.data}</div>;
});
```

使用`memo`的好处是，当组件依赖的数据变了，组件才会执行。

## `useMemo`

- 第一个参数是`() => value`
- 第二个参数是依赖`[m,n]`
- 只有当依赖变化时，才会计算出新的`value`
- 如果依赖不变，那么就重用之前的`value`

```ts
function App() {
  const [n, setN] = React.useState(0);
  const [m, setM] = React.useState(0);
  const onClick = () => {
    setN(n + 1);
  };
  const onClickChild = () => {};
  return (
    <div className="App">
      <div>
        <button onClick={onClick}>update n {n}</button>
      </div>
      <Child data={m} onClick={onClickChild} />
    </div>
  );
}

const Child = React.memo(props => {
  console.log("child 执行了");
  return <div onClick={props.onClick}>child: {props.data}</div>;
});
```

当点击按钮时，`App`重新执行了，导致`onClickChild`函数会被执行。因为函数是引用类型，每次执行都会生成一个新地址，所以`Child`就会重新执行。

使用`useMemo`将`onClickChild`包装一下，就可以了，当`m`变化时，执行`Child`组件

```ts
function App() {
  const [n, setN] = React.useState(0);
  const [m, setM] = React.useState(0);
  const onClick = () => {
    setN(n + 1);
  };
  const onClickChild = useMemo(() => () => {}, [m]);
  return (
    <div className="App">
      <div>
        <button onClick={onClick}>update n {n}</button>
      </div>
      <Child data={m} onClick={onClickChild} />
    </div>
  );
}

const Child = React.memo(props => {
  console.log("child 执行了");
  return <div onClick={props.onClick}>child: {props.data}</div>;
});
```

`注意`：
- 如果你的`value`是一个函数，那么你就要写成`() => (x) => console.log(x)`，这是一个返回函数的函数，特别奇怪，这就有了`useCallback`

### `useCallback`

```ts
const onClickChild = useMemo(() => () => {}, [m]);
/* 上下等价 */
const onClickChild = useCakkback(() => {}, [m]);
```

## `useRef`

- 如果需要一个值，在组件不断`render`时保持不变
- 初始化：`const count = useRef(0)`
- 读取：`const.current`
- 为什么需要`current`？
  - 为了保证两次`useRef`是同一个值，只有引用能做到
  
```ts
function App() {
  const [n, setN] = React.useState(0);
  const count = useRef(0);      // { current: 0 }
  const onClick = () => {
    setN(n + 1);
  };
  useEffect(() => {
    count.current += 1;     // 不能执行操作 count，count 实际是个对象
    console.log(count.current);
  });
  return (
    <div className="App">
      <div>
        <button onClick={onClick}>update n {n}</button>
      </div>
    </div>
  );
}
```

- `useState/useRenducer`每次都变
- `useMemo/useCallback`有条件的变
- `useRef`永远不变

## `forwardRef`

- 让函数组件支持`ref`
  - `props`无法传递`ref`属性
  - 实现`ref`的传递
- `useRef`
  - 可以用来引用`DOM`对象
  - 也可以用来引用普通对象
- `forwordRef`
  - 由于`props`不包含`ref`，所以需要`forwardRef`
  - 为什么`props`不包含`ref`，因为大部分的时候不需要用到

```ts
function App() {
  const buttonRef = useRef(null);
  return (
    <div className="App">
      <div>
        <Button ref={buttonRef}>按钮</Button>
      </div>
    </div>
  );
}
const Button = forwardRef((props, ref) => {
  console.log(props)
  console.log(ref)
  return <button className="red" ref={ref} {...props} />;
});
```

## `useImperativeHandle` 

```ts
function App() {
  const buttonRef = useRef(null);
  useEffect(() => {
    console.log(buttonRef.current);
  });
  return (
    <div className="App">
      <div>
        <Button ref={buttonRef}>按钮</Button>
      </div>
    </div>
  );
}
const Button = forwardRef((props, ref) => {
  const realButton = createRef(null);
  useImperativeHandle(ref, () => ({
    x: () => {
      realButton.current.remove();
    }
  }));
  return <button ref={ref} {...props} />;
});
```

## 自定义`Hook`

- 封装数据操作
- 还可以在自定义`Hook`里使用`Context`

## `stale closure`陈旧的闭包

```ts
function createIncrementFixed(i){
  let value = 0;
  function increment(){
    value += i;
    console.log(value)
    const message = `Current value is ${value}`
    return function logValue(){
      console.log(message)
    }
  }
  return increment;
}
const inc = createIncrementFixed(1)
const log = inc()   // logs 1
inc()               // logs 2
inc()               // logs 3
// works
log()               // logs "Current value is 1"
```

下面是解决方法：

```ts
function createIncrementFixed(i){
  let value = 0;
  function increment(){
    value += i;
    console.log(value)
    return function logValue(){
      const message = `Current value is ${value}`
      console.log(message)
    }
  }
  return increment;
}
const inc = createIncrementFixed(1)
const log = inc()   // logs 1
inc()               // logs 2
inc()               // logs 3
// works
log()               // logs "Current value is 3"
```