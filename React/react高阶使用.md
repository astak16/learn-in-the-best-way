## 非受控组件

无法使用 `setState` 获得想要的结果时使用非受控组件，比如文件上传

## Portal

作用：让组件渲染到父组件以外

使用场景：

- `overflow: hidden`
- 父组件 `z-index` 值太小
- `fixed` 放在 `body` 的第一层级

```jsx
ReactDOM.createPortal(
  <div className="modal">{this.props.children}</div>,
  document.body
);
```

## Context

```jsx
const ThemeContext = React.createContext('light')

<ThemeContext.Provider value={this.state.theme}>
	<A />
</ThemeContext.Provider>

// class组件：A 组件
class A extends React.Component {
	// static contextType = ThemeContext   等价于下面的 A.contextType = ThemeContext
	render() {
		const theme = this.context  // React 会往上找最近的 theme Provider
		return <div>{theme}</div>
	}
}
A.contextType = ThemeContext  // 指定 contextType 读取当前的 ThemeContext

// 函数组件：B 组件
function B = () => {
	// 函数组件没有 this.context
	// 函数组件通过可以使用 ThemeContext.Consumer
	return <ThemeContext.Consumer>
		{value => {value}}
	</ThemeContext.Consumer>
}
```

## 异步组件

```jsx
const A = React.lazy(() => import("./A"))

// 使用异步组件时，可能会有等待的一些情况，就可以使用 React.Suspense, fallback 可以传入 loading 组件
<React.Suspense fallback={<div>loading...</div>}>
	<A />
</React.Suspense>
```

## 性能优化

`react` 默认父组件有更新，子组件无条件更新

`shouldComponentUpdate` 默认返回 `true`

```jsx
shouldComponentUpdate(nextProps, nextState) {
	if(nextState.count !== this.setState.count) {
		return true  // 可以渲染
	}
	return false   // 不重复渲染
}
```

`PureComponent` 和 `memo` 实现了 `shouldComponentUpdate` 浅比较

`class` 组件使用 `PureComponent` ，函数组件使用 `memo`

`memo` 使用

```jsx
function A(props) {
  // 使用 props 渲染
}

function areEqual(prevProps, nextProps) {
  /*
   * 如果把 nextProps 传入 render 方法的返回结果与将 prevProps 传入 render 方法的返回结果一致则返回 true，否则返回 false
   */
}

React.memo(a, areEqual);
```

## 高阶组件

```jsx
// 高阶组件不是一种功能，而是一种模式
const HOCFactory = (Component) => {
  class HOC extends React.Component {
    // 在此定义多个组件的公共逻辑
    render() {
      return <Component {...this.props} />;
    }
  }
  return HOC;
};
const EnhancedComponent1 = HOCFactory(WrapperComponent);
```

## Render Props

```jsx
// Render Props 的核心思想
// 通过一个函数将 class 组件的 state 作为 props 传递给纯函数组件
class Factory extends React.Component {
  constructor() {
    this.state = {
      // state 即多个组件的公共逻辑数据
    };
  }

  // 修改 state
  render() {
    return <div>{this.props.render(this.state)}</div>;
  }
}

const App = () => {
  <Factory
    render={
      /* render 是个函数组件 */
      (props) => (
        <p>
          {props.a}
          {props.b}
          ...
        </p>
      )
    }
  />;
};
```
