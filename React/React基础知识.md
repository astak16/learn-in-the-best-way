## `React.FunctionComponent`

`React`提供了一个组件类型`React.FunctionComponent`，可简写`React.FC`，

- 可以接收一个泛型`p`，默认是`{}`
- `children`，返回一个`React.ReactNode`，这个`children`是任何`component`都拥有的
- 静态属性`defaultProps`，组件的默认属性，外部可以不传这个属性。

```ts
interface IHelloProps {
  message?: string;
}

const Hello: React.FunctionComponent<IHelloProps> = (props) => {
  return <h2>{props.message}</h2>;
};

Hello.defaultProps = {
  message: "Hello  world",
};
```

## `React Hook`

- 完全可选
- 百分百向后兼容
- 没有计划从`React`移除`class`

`Hook`是一个特殊的函数，它可以让你勾入`React`特性，例如`useState`就允许在`React`函数组件添加`state Hook`。

在编写函数组件时，意识到要向里面添加一些`State`时，以前的做法是必须转换成`Class`类型的组件，现在可以在现有的函数组件中使用`Hook`

## `useState`

分开使用

```ts
import React, { useState } from "react";

const LikeButton: React.FC = () => {
  const [like, setLike] = useState(0);
  const [on, setOn] = useState(true);
  return (
    <>
      <button
        onClick={() => {
          setLike(like + 1);
        }}
      >
        {like}👍
      </button>
      <button
        onClick={() => {
          setOn(!on);
        }}
      >
        {on ? "ON" : "OFF"}
      </button>
    </>
  );
};

export default LikeButton;
```

合在一起使用

```ts
import React, { useState } from "react";

const LikeButton: React.FC = () => {
  const [obj, setObj] = useState({ like: 1, on: true });
  return (
    <>
      <button
        onClick={() => {
          setObj({ like: obj.like + 1, on: obj.on });
        }}
      >
        {obj.like}👍
      </button>
      <button
        onClick={() => {
          setObj({ like: obj.like, on: !obj.on });
        }}
      >
        {obj.on ? "ON" : "OFF"}
      </button>
    </>
  );
};

export default LikeButton;
```

## 删除数据

`react` 中有一个概念 `immutable` ，意思是不允许 `state` 有任何的改变。

一旦修改 `state` ，后面做 react 性能做优化的时候会有问题。

```jsx
const { list } = [...this.state.list];
list.splice(1, 1);
this.setState({ list });

// 不推荐
this.state.list.splice(1, 1);
this.setState({ list: this.state.list });
```

## 属性名

`label` 的 `for` 属性，在 `jsx` 中会和 `for` 循环冲突， `react` 用 `htmlFor` 来代替 `label` 标签的 `for`

```jsx
<label htmlFor='insertArea'>姓名</label>
<input type='text' id='insertArea'/>
```

## 父组件和子组件通信

子组件操作父组件的数据：

1. 父组件向子组件传递一个方法
2. 子组件调用这个方法，间接的操作父组件的数据

```jsx
class TodoList extends Component {
  deleteItem = (number) => {
    alert(number);
  };
  render() {
    return <TodoItem deleteItem={this.deleteItem} />;
  }
}

class TodoItem extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return <div onClick={this.props.deleteItem(1)}>按钮</div>;
  }
}
```

## setState 接收函数

```jsx
onClick(e) {
	const value = e.target.value
	this.setState(() => ({
		value
	}))
}

onClick() {
	// prevState 是修改前的数据状态
	this.setState((prevState) => ({
		list: [...prevState.list]
	}))
}
```

## PropTypes

限制父组件向子组件传值的类型

```jsx
import PropTypes from "prop-types";
TodoItem.propTypes = {
  test: PropTypes.string.isRequired,
  content: PropTypes.oneOfType([PropTypes.string, PropTypes.number]),
  deleteItem: PropTypes.func,
  index: PropTypes.number,
};
```

## defaultProps

属性默认值

```jsx
TodoItem.defaultProps = {
  test: "hello world",
};
```

## 虚拟 DOM

当父组件的 `render` 函数被运行时，它的子组件的 `render` 也会被重新执行

1. state 数数据
2. JSX 模版
3. 数据 + 模版 生成虚拟 DOM（虚拟 DOM 就是一个 js 对象，用它来描述正式的 DOM），（损耗了性能）。

   ```jsx
   ["div", { id: "name" }, ["span", {}, "hello world"]];
   ```

4. 用虚拟 DOM 的结构生成真实的 DOM，来显示

   ```html
   <div id="abc"><span>abc</span></div>
   ```

5. state 发生变化（setState 时，数据会发生变化）
6. 数据 + 模版 生成新的虚拟 DOM（极大提升了性能），diff 算法，react 中的 diff 是同层比较，如果第一层节点就发生了变化，下面就不在比较了，直接全部替换

   ```jsx
   ["div", { id: "name" }, ["span", {}, "bye bye"]];
   ```

7. 比较原始虚拟 DOM 和新的虚拟 DOM 的区别，找到区别的是 span 中的内容（极大提升了性能）
8. 直接操作 DOM，改变 span 中的内容

## Ref

react 建议用数据驱动，尽量不要去操作 DOM

```jsx
<input onChange={this.onChange} ref={(input) => (this.input = input)} />;

onChange = (e) => {
  const value = this.input.value;
  // 等价于
  const value = e.target.value;
};
```

## 声明周期函数

- componentWillMount：组件在即将被挂载到页面时执行（第一次挂载的时候被执行）
- render
- componentDidMount：组件在被挂载到页面后执行（第一次挂载的时候被执行），ajax 请求最好发在这里
- shouldComponentUpdate：组件被更新之前执行（需要在函数内部返回 true，返回 false 不会执行）
- componentWillUpdate：组件被更新之前执行，但在 shouldComponentUpdate 之后（shouldComponentUpdate 返回 true 才会被执行）
- componentDidUpdate：组件更新完之后执行
- componentWillReceiveProps：没有 props 参数，不会被执行
  1. 一个组件要从父组件接收参数
  2. 只要父组件的 render 函数被重新执行了，子组件的 componentWillReceiveProps 就会被执行（如果子组件第一次出现在页面中，不会执行）
- componentWillUnmount：组件被移除前执行

### shouldComponentUpdate

```jsx
/**
 * nextProps 一个组件更新时 props 会变化成什么样
 * nextState 一个组件更新时 state 会变化成什么样
 */
shouldComponentUpdate(nextProps, nextState) {
	if(nextProps.content !== this.props.content) {
		return true
	} else {
		return false
	}
}
```

## 动画

安装 `react-transition-group`

```jsx
import { CSSTransition } from "react-transtion-group";

<CSSTransition
  in={this.stata.show} // 告诉 CSSTransition 组件什么时候有动画
  timeout={1000} // 动画时长 1s
  className="fade"
  unmountOnExit // 元素消失后会被移除
  appear={true} // 页面一刷新，也会有动画效果
>
  <div>hello</div>
</CSSTransition>;
```

- fade-enter（动画进入前一瞬间）
- fade-enter-active（动画进入前第二瞬间，一直到动画完成）
- fade-enter-done（进入动画完成时）
- fade-exit（动画消失前一瞬间）
- fade-exit-active（动画消失前第二瞬间，一直到动画消失完成）
- fade-exit-done（消失动画完成时）
- fade-appear（动画进入前一瞬间）
- fade-appear-active（动画进入前第二瞬间，一直到动画完成）
