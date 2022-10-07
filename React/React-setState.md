`setState`特点

- 不可变值
- 可能是异步更新
- 可能会被合并

`state` 要在构造函数中定义

## 不可变值

函数式编程思想，纯函数，不能有副作用

什么时候修改，什么时候对 `state` 操作，不要提前操作

```jsx
// 操作数组
this.setState({
  list1: this.state.list1.concat(100),
  list2: [...this.state.list2, 100],
  list3: this.state.list3.slice(0, 3),
  list4: this.state.list4.filter((item) => item > 100),
  list5: list5Copy, // 其他操作
});
```

不能直接对数组进行 `pop` 、 `push` 、 `splice` 等操作

```jsx
// 操作对象
this.setState({
	obj1: Object.assign({}, this.state.obj1, {a: 100}),
	obj2: {...this.state.obj2, {a: 100}}
})
```

不能直接对 `this.state.obj1` 操作

为什么要强调不可变值，因为在性能优化部分 `shouldComponentUpdate` 是比较两个值 `oldState` 和 `newState` 值是否一样， `setState` 会触发 `shouldComponentUpdate` 此时 `oldState` 和 `newState` 的值就一样了。

## 可能是异步更新

在异步函数中使用 `setState` 是一个同步操作，否则为异步操作

```jsx
// 异步
this.state = {
  count: 0,
};

this.setState({
  count: this.state.count + 1,
});

console.log(this.state.count); // 0
```

```jsx
// 同步
this.state = {
  count: 0,
};

setTimeout(() => {
  this.setState({
    count: this.state.count + 1,
  });
  console.log(this.state.count); // 1
}, 0);
```

```jsx
// 同步
this.state = {
  count: 0,
};

document.body.addEventListener("click", () => {
  this.setState({
    count: this.state.count + 1,
  });
  console.log(this.state.count); // 1
});
```

## 可能会被合并

`setState` 传入的是对象就会被合并，传入函数就不会被合并

```jsx
// 会被合并
this.setState({
  count: this.state.count + 1,
});
this.setState({
  count: this.state.count + 1,
});
this.setState({
  count: this.state.count + 1,
});

count; // 1
```

```jsx
// 不会被合并
this.setState((prevState, props) => {
  return {
    count: prevState.count + 1,
  };
});
this.setState((prevState, props) => {
  return {
    count: prevState.count + 1,
  };
});
this.setState((prevState, props) => {
  return {
    count: prevState.count + 1,
  };
});
count; // 3
```

## setState 主流程

`this.setState(newState)` → `newState` 存入 `padding` 队列 → 是否处于 `batchUpdate` 机制中

→ 是 → 保持组件与 `dirtyComponents` 中

→ 否 → 遍历所有的 `dirtyComponents` → 调用 `updateComponent` → 更新 `padding state or props`

函数在开始执行之前它会先设置一个变量 `isBatchingUpdates = true` ， 这是 `react` 的一个机制，然后这函数执行完后在将 `isBatchingUpdates` 改为 `false` 。

这个变量的设置不是在函数里面的，可以认为先设置把这个变量设为 `true` 然后执行这个函数，函数执行完后把这个变量在改为 `false`

```jsx
increate = () => {
  // 开始：处于 batchUpdate
  // isBatchingUpdates = true
  this.setState({
    count: 1,
  });
  // 结束
  // isBatchingUpdate = false
};
```

```jsx
increate = () => {
  // 开始：处于 batchUpdate
  // isBatchingUpdates = true
  setTimeout(() => {
    // 此时 isBatchingUpdates 为 false
    this.setState({
      count: 1,
    });
  }, 0);
  // 结束
  // isBatchingUpdates = false
};
```

## setState 异步还是同步？

- `setState` 无所谓异步还是同步
- 看是否能命中 `batchUpdate` 机制
- 依据是判断： `isBatchingUpdates`

## batchUpdate 机制

`React` 可以“管理”的入口

- 生命周期（和它调用的函数）
- `React` 中注册的事件（和它调用的函数）

`React` “管不到”的入口

- `setTimeout` 、 `setInterval` 等（和它调用的函数）
- 自定义的 `dom` 事件（和它调用的函数）

## transaction 事务机制

定义一个开始的逻辑，一个结束的逻辑。

在执行这个的时候，先执行开始的逻辑，再执行当前的函数，最后在执行结束的逻辑，这种机制就是 `transaction` 事务机制

这个开始和结束的逻辑不一定在函数中定义

```jsx
increate = () => {
  // 开始：处于 batchUpdate
  // isBatchingUpdates = true
  // 其他任何操作
  // 结束：isBatchingUpdates = false
};
```
