## `<a>`标签

`<a>`标签通过设置`href`属性，可以跳转到同网页的其他位置、外部网页、打电话或发送电子邮件。

`<a>`标签常用的属性有 `href` `target`

### `href`

`href`的取值可以是：“网址”、“路径”，“伪协议”、“锚点”

网址：
* http://google.com
* https://google.com
* //google.com

网址一般有三种形式，分别是：`http://`、`https://`、`//`。使用`//`协议，当前页面是什么协议就是网址就是什么协议。

路径：
* 当前路径
* 绝对路径

使用绝对路径的话，如果在`file`协议中打开，会找不到这个文件，因为它会去当前服务的根目录去找，而根目录是没有这个文件的。

伪协议：
* `mailto:${邮箱}`
* `tel:${电话}`
* `tencent://message/?uin=${QQ号码}&Site=400301.com&Menu=yes`

锚点：页面中`id`名称，点击后调至对应的内容
```html
<a href="#1"></a>
<div id=1></div>
<div id=2></div>
```

### `target`

`target`有取值分别是：`_blank`、`_self`、`_parent`、`_top`、自定义值。默认取值`_self`。

* `_blank`在新标签中打开
* `_parent`在当前页面中的的父页面中打开
* `_top`在当前页面的最顶层的页面中打开

在只有一层页面时，`_top`和`_parent`效果和`_self`相同；在只有两层页面时，`_parent`和`_top`效果相同；只有在超过三层页面时，才能看出他们之间的区别。

自定义取值是什么意思呢？

```js
<a src="https://google.com" target="tag">点我</a>
<a src="https://baidu.com" target="tag">点我</a>
```
上面这段代码，当点击第一个`<a>`标签时，将会在一个叫做`tag`的页面中打开，发现和`target='_blank'`没啥区别。这时在点击第二个`<a>`标签，你会发现它会在第一个打开的页面中打开了第二个页面。

## `<table>`标签

`<table>`标签内部直接子标签有`<thead>`、`<tbody>`、`<tfoot>`、`<caption>`。

一般在`<thead>`标签内是`<tr>`、`<th>`；`<tbody>`标签`<tr>`、`<td>`。`<tfoot>`标签一般可以省略。

```html
<table>
  <caption>名单</caption>
  <thead>
    <tr>
      <th>姓名</th>
      <th>学号</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>张三</td>
      <td>1</td>
    </tr>
    <tr>
      <td>李四</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
```

使用`<table>`制作表格是行与行之间会有空隙，优势特别难受，可以使用`CSS`样式消除它

```css
tr,td{
    border-collage: collage;
    border-spacing: 0;
}
```

## `<img>`标签

`<img>`标签在页面加载时，会发送`get`，请求。同时它有两个事件`onload`和`onerror`事件，图片加载成功会调用`onload`事件，图片加载失败会调用`onerror`事件。

`<img>`标签默认有两个属性：`src`、`alt`。

`src`取值可以是路径或者外部网址。`alt`属性在图片加载失败时，会在页面中显示出字来，用来提示用户这是一张什么图片。

`<img>`标签和一段文字组成的`figure`语法可以用`<figure>`标签。用`<figure>`标签包裹图片文字，用`<figcaption>`包裹文字。

```html
<figure>
    <img src="./img.png">
    <figcaption>这是一张图片</figcaption>
</figure>
```