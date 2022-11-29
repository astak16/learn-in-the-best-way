## 实现私有属性

### 用闭包实现私有属性

```js
let WidgetA;
(function () {
  WidgetA = function () {};
  let appName = "widgetA";
  WidgetA.prototype.getAppName = function () {
    return appName;
  };
})();

let widgetA = new WidgetA();
console.log(widgetA.appName); // undefined
console.log(widgetA.getAppName()); // "widgetA"
```

### 用构造函数实现私有属性

```js
function WidgetB() {
  let appName = "widgetB";
  this.getAppName = function () {
    return appName;
  };
}

let widgetB = new WidgetB();
console.log(widgetB.appName); // undefined
console.log(widgetB.getAppName()); // "widgetB"
```

#### 通过原型实现私有属性

```js
function WidgetC() {}
WidgetC.prototype = (function () {
  let appName = "widgetC";
  return {
    getAppName: function () {
      return appName;
    },
  };
})();

let widgetC = new WidgetC();
console.log(widgetC.appName); // undefined
console.log(widgetC.getAppName()); // "widgetC"
```

### 用 WeakMap 实现私有属性

```js
let WidgetE;
{
  let privateProp = new WeakMap();
  WidgetE = function () {
    privateProp.set(this, { appName: "widgetE" });
  };
  WidgetE.prototype.getAppName = function () {
    return privateProp.get(this).appName;
  };
}

let widgetE = new WidgetE();
console.log(widgetE.appName); // undefined
console.log(widgetE.getAppName()); // "widgetE"
```

### 用 symbol 实现私有属性

```js
let WidgetF;
{
  let privateProp = Symbol();
  WidgetF = function () {
    this[privateProp] = { appName: "widgetF" };
  };
  WidgetF.prototype.getAppName = function () {
    return this[privateProp].appName;
  };
}

let widgetF = new WidgetF();
console.log(widgetF.appName); // undefined
console.log(widgetF.getAppName()); // "widgetF"
```

## 实现静态私有属性

### 用闭包实现

```js
let WidgetG;
(function () {
  WidgetG = function () {};
  let appName = "widgetG";
  WidgetG.getAppName = function () {
    return appName;
  };
})();

console.log(WidgetG.appName); // undefined
console.log(WidgetG.getAppName()); // "widgetG"
```

### 用 Symbol 实现

```js
let WidgetH;
{
  let privateProp = Symbol();
  WidgetH = function () {};
  WidgetH[privateProp] = { appName: "widgetH" };
  WidgetH.getAppName = function () {
    return this[privateProp].appName;
  };
}
console.log(WidgetH.appName); // undefined
console.log(WidgetH.getAppName()); // "widgetH"
```

## super 实现

```js
function WidgetJ() {
  this.appName = "widgetJ";
}
WidgetJ.prototype.getAppName = function () {
  return this.appName;
};

function WidgetK() {
  let widgetJ = new WidgetJ(); // 调用 super
  this.__proto__.__proto__ = widgetJ.__proto__; // WidgetK.prototype.__proto__ = WidgetJ.prototype
  for (let key in widgetJ) {
    if (widgetJ.hasOwnProperty(key)) {
      this[key] = widgetJ[key];
    }
  }
  this.appName1 = "widgetK";
}
WidgetK.prototype.getAppName1 = function () {
  return this.appName1;
};
let widgetK = new WidgetK();
console.log(widgetK.getAppName()); // "widgetJ"
console.log(widgetK.getAppName1()); // "widgetK"
```

## Object.create 实现

```js
function WidgetL() {
  this.appName = "widgetL";
}

function objectCreate(origan) {
  let obj = {};
  obj.__proto__ = origan;
  return obj;
}

let widget = objectCreate({});
widget.__proto__; // null
```

## new 实现

```js
function NEW(fn) {
  let o = { __proto__: fn.prototype };
  fn.apply(o, arguments);
  return o;
}
```

## 总结

1. 构造函数的原型叫做 `prototype`
2. 对象的原型叫做 `__proto__`
3. `对象.__proto__ === 构造函数.prototype`

顺便说一句，`Function` 即是对应也是函数（构造函数）

所以 `Function.__proto__ === Function.prototype`
