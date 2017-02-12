# 总结
- 块级元素

  `<h> <p> <ul> <li>`
- 内联元素

  `<a> <b> <em> <img>`

- 将文本和元素集中在一个快级元素中
  `<div>`

- 将文本和元素集中在一个内联元素中
  `<span>`

## 文本
`<b>`
`<strong>`粗体

`<em>`斜体

`<sup>`上标

`<sub>`下标

`<br />`换行

`<hr />`分隔线

引用

- `<blockquote cite="URL">`长引用，会对整段内容进行缩进。
- `<q>`短引用，也可用`cite`

`<abbr title="">`缩写词或首字母缩写词

`<cite>`引用作品，在HTML5不可以用在人名，浏览器中会显示为斜体。

`<dfn>`新术语的定义

`<address>`设计者详细信息

`<ins>` 显示已经插入到文档的内容，通常是下划线

`<del>`删除线

`<s>`表示不准确或不相关但是不应当删除的内容，显示为从中间穿过的线条

## 列表

`<ul>`有序

`<ol>`无序

自定义列表
```html
<dl>
  <dt>被定义的术语</dt>
  <dd>定义</dd>
</dl>
```

## 链接

`<a href="绝对URL/相对URL">`

`<a href="mailto:xxx@ddd.cc">`Email 链接

`target="_blank"`在新窗口打开链接

`<a href="#id_name">`链接到当前页面的特定位置

`<a href="../index.html>"`父文件夹链接

`<a href="../../index.html>"`祖父文件夹链接

## 图像
`<img src="URL" alt="" title="" width="" height=""/>`

 照片最好保存为`JPEG`格式；使用单色的插图或徽标应保存为`GIF`格式

块级元素`h p`都是另起一行的。

`HTML5`中对图片使用
- `<figure>`包含图像以及图像的说明
- `<figcaption>`给图像添加说明

## 表格
`<table>`

`<tr>`table row

`<td>`table date
  - `colspan="x"`跨x列
  - `rowspan="x"`跨x行

`<th>`table heading
 - `scope="row"`行标题
 - `scope="col"`列标题

`<thead><tbody><tfoot>`用于长表格
## 表单

`<form action="服务器URL" method="">`表单结构

`method`
- `get`
 - 短表单
 - 只从Web服务器检索数据的情形
- `post`
 - 允许用户上传文件
 - 表单很长
 - 包含敏感信息
 - 从数据库交互信息


 `<input type="text" name="" maxlength="" />`单行文本框

 `<input type="password" name="" maxlength="" />`密码框

 `<textarea name="" cols="" rows="">`文本域

 `<input type="radio" name="" value="" check="" />`单选按钮

 `<input type="checkbox" name="" value="" check="" />`复选框

下拉列表框
 ```html
 <section name="">
   <option value="aa">aa</option>
   <option value="bb">bb</option>
   <option value="cc">cc</option>
 </section>
 ```
`selected`默认选中

多选框
```html
<select name="" size="" multiple="multiple">
  <option value="aa">aa</option>
  <option value="bb">bb</option>
  <option value="cc">cc</option>
</select>
```

文件上传和提交

```html
<form  action="index.html" method="post">
  <input type="file" name="" value="" />
  <input type="submit" name="" value="" />
</form>
```
以图像做提交按钮
```html
<input type="img" src="" width="" height="" />
```
按钮和隐藏控件
```html
<button><img src="" alt="aa" width="" height="">xx</button>
<input type="hidden" name="" value="" />
```


`<label for="">`标签表单控件

`<fieldset><legend>`组合表单元素

`HTML5`表单
- `<input type="data" name=""/>`日期控件
- `<input type="email" name="" />`电子邮件
-  `<input type="url" name="" />`URL
- `<input type="search" name="" placeholder="xxx" />`搜索输入控件


## `HTML5`布局

页眉页脚`<header> <footer>`

导航`<nav>`

文章`<article>`

附属信息`<aside>`

部分`<section>`

标题组`<hgroup>`
