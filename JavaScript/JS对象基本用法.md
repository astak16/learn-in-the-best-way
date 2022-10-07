## 对象声明

`js`对象声明有两总方式：字面量声明和构造函数声明

### 字面量声明

1. 在创建的时候添加属性或者方法
```js
let obj = {
  name: 'jack',
  age: 28,
  say() {
    console.log(`My name is ${this.name}.`)
  }
}
```

2. 先创建一个空对象，后面在一个个的添加属性和方法
```js
let obj = {}
obj.name = 'jack'
obj.age = 28
obj.say = () => { console.log('大家好')}
```

PS: 这个空对象并不是真正意义上的空对象，它继承自`object`对象

### 构造方法声明

```js
let obj = new Object()
obj.name = 'jack'
obj.age = 28
obj.say = () => { console.log('大家好')}
```

## 删除对象属性

使用`delete`可以删除对象的属性
```js
let obj = { name: 'jack' }
delete obj.name   //{}
```

`delete`不能用作删除数组的某一项，被删除的那一项只是将它置空，`key`没有重新排序，`length`也没有变化。
```js
let arr = [1, 2, 3, 4]
delete arr[2]
arr.length      //4
arr.[2]         //undefined
```

## 查看对象的属性

```js
let obj = { name: 'jack'}
obj.name    //'jack'
```

## 增加或者修改对象属性

```js
let obj = {
  name: 'jack'
}
obj.age = 28     //增加属性
obj.name = 'uccs'   //修改属性
```

## `'name' in obj`和`obj.hasOwnProperty('name')`的区别

使用`in`操作符，可以查看一个属性在不在对象中
```js
let obj = { name: 'jack' }
'name' in obj     //true
'age' in obj      //false
```

但是`in`操作符，不能确定这个属性是它自身的还是它原型上的
```js
let obj = { name: 'jack' }
let obj2 = Object.create(obj)
'name' in obj       //true
'name' in obj2      //true
```

要准确知道一个属性是否是它自身属性，需要使用`hasOwnProperty()`方法
```js
let obj = { name: 'jack' }
let obj2 = Object.create(obj)
obj.hasOwnProperty('name')      //true
obj2.hasOwnProperty('name')     //false
```