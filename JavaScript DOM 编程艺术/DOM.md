# DOM

## 什么是`DOM`
`D`--`document`


`O`--对象
- 用户定义对象
- 内建对象
- 宿主对象

`M`--`Model`

## 节点

元素节点，文本节点，属性节点

## 获取元素

`getElementById`

`getElementsByTagName`

`getElementsByClassName`

`childNodes`获取任何一个元素的所有子元素.返回的数组包含所有类型的节点，而不仅仅是元素节点。
- `childNodes[0]=firstChild`
- `childNodes[node.childNodes.length-1]=lastChild`

## 元素属性

`nodeType`
- 元素节点的属性为1
- 属性节点的属性为2
- 文本节点的属性为3

`nodeValue`得到或设置文本节点的值
## 获取和设置属性
`getAttribute`

`setAttribute`

## 事件处理函数

事件处理函数的机制：

1. 给某个元素添加了事件处理函数后，一旦事件发生，相应的JavaScript代码会执行。
2. 被调用的JavaScript代码可以返回一个值，这个值会传递给那个事件处理函数

`Example:`
>给一个连接添加了事件处理函数`onclick`后，如果`JavaScript`代码执行后返回`true`，`onclick`事件处理函数会认为‘这个链接被点击了’。

`onload`事件处理程序
```javascript
function addLoadEvent(func){
  var oldonload = window.onload;
  if（typeof window.onload != 'function'){
    window.onload = func;
  }else{
    window.onload = function(){
      oldonload();
      func();
    }
  }
}
```
