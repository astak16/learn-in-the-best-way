## 语句和表达式

表达式和语句的区别在于，语句是为了进行某种操作，一般情况下不需要返回值，而表达式是为了得到值。

一般情况下，在`javascript`中每一行就是一个语句。

```js
let a = 1 + 3
```

这是一条赋值语句，其中`1+3`就是语句。


## 标识符

在`javascript`中，第一个字符可以是`Unicode`字符或或者`$`或者`_`或者中文。后面的字符，除了上面所说的，还可以是数字。

合法标识符：

```js
let _ = 1
let $ = 2
let $3 = 3
let 名字 = 'zhangsan'
```

非法标识符：

```js
let int = 1                 //保留字  
let 90 = 43                 //数字开头
let hello world = '你好'      //标识符中有空格
```

## 条件语句

编程语言中，代码需要依靠不同的输入做出相应的决定，就需要用到条件语句。

基本语法：

```js
if (condition) {
    ...
} else {
    ...
}
```

`else`和后面花括号中的语句不是必须的，

```js
if (condition){
    ...
}
...
```

这里要注意的是，这种情况下，后面的语句是一定会执行的。

如果条件条件比较多的情况可以配合`else if`使用

```js
if (condition) {
    ...
} else if (condition2) {
    ...
} else {
    ...
}
```

当对应的`condition`被满足时，就会执行对应花括号中的语句。如果都不满足，会执行最后的`else`语句。

## `while`和`for`

`while`语句是，当满足条件就会执行花括号中的代码。

```js
let i = 1
while (i < 10) {
    console.log(i)
    i++
}
```

`for`语句是`while`语句的语法糖

```js
for(let i = 0; i < 10; i++){
    console.log(i)
}
```

`while`和`for`语句都是由三部分组成：

* 声明语句
* 条件语句
* 循环体

## `break`和`continue`

`break`可以跳出当前循环
```js
for(let i = 0; i < 10; i++){
    if(i === 5) break
    console.log(i)
}

//0
//1
//2
//3
//4
```

Tips：如果有多层循环就跳不出整个循环了。

```js
for(let i = 0; i < 10; i++){
    for(let j = 0; j < 10; j++){
        if(j === 5) break
    }
    console.log(i)
}
```

`continue`会跳出循环中的满足条件的那一次

```js
for(let i = 0; i < 10; i++){
    if(i % 2 !== 0) continue
    console.log(i)
}

//0
//2
//4
//6
//8
```

## `label`

`lable`表示是代码块的意思

```js
foo: {
    console.log(1)
}
```

大括号内的这块表示是`foo`。

上面讲到`break`只能跳出当前循环，不过可以配合`label`使用，跳出你想跳出的循环。

```js
loop1:
for(let i = 0; i < 10; i++){
    for(let j = 0; j < 10; j++){
        if(i===5 && j===5) break loop1;
        console.log('i: ' + i,'j: '+ j)
    }
}
```

`label`也可以配合`continue`使用，用法是一样的。
