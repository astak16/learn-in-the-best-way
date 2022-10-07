## `HTML` 定义

`HTML`是超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言，`HTML`是一种技术与`CSS`和`JS`一起被众多网站用于设计网页、网页应用程序以及移动应用程序的用户界面。`HTML`是构建网站的基石。

## `HTML`历史

* 1989年，李爵士提出一个基于互联网的超文本系统
* 1990年，李爵士规定HTML并写出浏览器和服务器软件
* 1991年，`HTML`正式诞生，有 18 个元素
* 1993年，发布首个HTML规范的提案，NCSA Mosaic浏览器支持`img`标签，可以在网页上浏览图片
* 1994年10月，W3C 成立
* 1995年11月，`HTML2.0`发布
* 1996年起，`HTML`规范由 W3C 维护
* 1997年初，`HTML3.2`发布
* 1997年底，`HTML4.0`发布
* 2014年，`HTML5`发布

## `HTML`起手式

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>

</body>
</html>
```

上面这段代码是`HTML`的起手式。

### `DOCTYPE`
第一句是`<!DOCTYPE html>`是文档类型声明，这句话是必须要的，它告诉浏览器请按照`HTML`的标准进行渲染。

### `<html>`
下面紧接着的是`<html>`标签，它是整个页面的根元素，所有的元素必须是它的后代。

它有个`lang="en"`的属性，它的作用的指定页面的语言，我们在浏览器上打开英文网页时，经常出现一个弹窗显示"是否需要翻译当前页面"，浏览器就是根据这个属性来判断的。

### `<head>`

`<head>`标签主要是用于放置元信息相关的标签，这些信息一般不是在页面中显示出来，主要是给浏览器（机器）阅读的。

它内部标签主要有`<title>`、`<meta>`、`<link>`、`<script>`，`<link>`和`<script>`这里先不讲解。

#### `<title>`

`<title>`标签和`<h1>`标签很容易被混淆的，都是用来表示标题的意思。

那他们最大的区别是什么呢？

`<title>`标签是页面的标题，`<h1>`是页面中某篇文章的标题。

我们在使用浏览器的收藏功能时，收藏的页面显示的标题就是`<title>`标签中的内容。

#### `<meta>`

`<meta>`标签是元数据标签，`<head>`标签中可以存在多个`<meta>`标签。

`<meta>`标签的属性有

* `charset`
* `name`
* `content`
* `http-equiv`

##### `chartset`

首先来看下`chartset`属性

```html
<meta charset="UTF-8">
```

`charset`属性描述了文章的编码方式，应该放置在`<head>`标签的第一个位置。因为在浏览器解析到它之前，页面中可能会存在乱码的现象。

一般情况，都是在`http`头信息中指定编码方式，但如果在`file`协议中，`charset`属性就显得非常重要了。

##### `name`

`name`是元数据的属性，它和`content`配合使用。

##### `http-equiv`

`http-equiv`和`http`头信息类似，用于渲染页面的需要遵守的规则，和`content`配合使用。

##### `content`

`content`是元数据的值，它的值可以是`name`属性对应的值，也可以是`http-equiv`属性对应的值。

##### `name`为`viewport`的`<meta>`

`name`为`viewport`是移动端开发的事实标准。

对于做了移动端适配的网页，已经不需要用户缩放查看了所以下面是一个标准的移动端开发的`<meta>`标签。

```html
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalbale=no">
```

* `width`：页面宽度，可以取值具体的数字，也可以是`device-width`，表示设备宽度相等
* `height`：页面高度，可以取值具体的数字，也可以是`device-height`，表示跟设备宽度相等
* `initial-scale`：初始缩放比例
* `minimum-scale`：最小缩放比例
* `maximun-scale`：最大缩放比例
* `user-scalable`：是否允许用户缩放

Tip：使用`vscode`，输入`!`并按下`tab`键，就可以一件创建`HTML`起手式

## `HTML`常用标签

### `<h1>~<h6>`

不同级别的标题，`<h1>`级别最高，`<h6>`级别最低。

### `<section>`

表示文档中一个独立的部分，没有特别的语义。

`<section>`标签和`<h1>~<h6>`标签嵌套使用，会出现特别好玩的事情——标题会自动下降一个等级。

```html
<h1>外面标题</h1>
<section>
  <h1>第一个标题</h1>
  <p>我第一个标题的内容</p>
  <section>
    <h1>第二个标题</h1>
    <p>我是第二个标题的内容</p>
    <section>
      <h1>第三个标题</h1>
      <p>我是第三个标题的内容</p>
    </section>
  </section>
</section>
```

显示效果如下：
![11.png](https://i.loli.net/2019/08/25/pQP9UHJcrk1yIF5.png)

### `<article>`

文章主体部分可以用`<article>`

### `<main>` 

`<main>`标签整个页面只出现一个，表示页面的主要内容，可以理解为特殊的`<div>`标签

### `<aside>`

文章边栏导航性质的内容可以使用`<aside>`标签


### `<pre>`、`<samp>`、`<code>`

`<pre>`标签表示预先排过版的，不需要浏览器在进行排版了。

输出一段示例可以用`<samp>`标签。

```html
<pre><sapm>
GET /home.html HTTP/1.1
Host: www.example.org
</samp></pre>
```

`<code>`在浏览器中用于代码展示

```html
<pre><code>
function fn(){
    console.log(1)
}
</code></pre>
```

使用`code`标签时需要注意，如果是`html`代码，尖括号要用转义符

```html
<pre><code>
  &lt;h1&gt;标题&lt;/h1&gt;
</code></pre>
```

### `<em>`、`<strong>`

在语义上`HTML`的一些标签就是必须存在的，其程度可以达到：**如果没有这个标签，文字会产生歧义的程度。**，比如`<em>`标签

```html
今天我吃了<em>一个</em>苹果。
今天我吃了一个<em>苹果</em>
```

`<em>`和`<strong>`的区别是：`<em>`标签表示重音，在语气上强调是“一个”还是“苹果”；`<strong>`只是单纯的表示加粗一下，没有语气上的重音。

### `<a>`

`<a>`标签通过设置，可以跳转到同网页的其他位置、外部网页、打电话或发送电子邮件。

`<a>`标签常用的属性有 `href` `target`

#### `href`

`href`的取值可以是：“网址”、“路径”，“伪协议”、“锚点”

伪协议：
* `mailto:${邮箱}`
* `tel:${电话}`
* `tencent://message/?uin=${QQ号码}&Site=400301.com&Menu=yes`

锚点：页面中`id`名称，点击后调至对应的内容
```
<a href="#1"></a>
<div id=1></div>
<div id=2></div>
```

### `HTML`的全局属性

全局属性是所有`HTML`元素共有的属性，它们可以用于所有元素。

常用的属性有：
* `class`
* `id`
* `title`
* `style`
* `tabindex`
* `contenteditable`
* `data-*`
* `dir`