---
title: JavaScript-ECMAScript(基础)
date: 2016-08-16 18:55:28
categories: 编程
tags: [JavaScript,ECMAScript]
---


# JavaScript在HTML中使用

- 内部嵌入
````html
<script type="text/javascript></script>
````
- 外部嵌入
````html
<script  src="example.js"></script>
````

注意：

- 第二个js脚本可能在第一个脚本之后进行覆盖，所以要确保不会发生冲突。
- 在内部嵌入的js脚本要写在`<body></body>`的末尾，为了在加载js时不影响阅读。







# JavaScript基本语法

### 标识符

`JS`的标识符是区分大小写，其标识符由字母、数字、下划线、`$`构成，标识符的第一个字母不能为数字。



### 注释

注释和C一样，分单行和多行。



### 数据类型

JS有6种数据类型：`Undefined`、`Null`、`Boolean`、`Number`、`String`、`Object`。

- undefined

  使用`var`声明变量但没有对其初始化

- Null

  `null`表示一个空对象指针，因此`typeof`检查`null`值时会返回`Object`。

- Boolean

  |   数据类型    |  转换为`true`的值   | 转换为`false`的值 |
  | :-------: | :------------: | :----------: |
  |  Boolean  |      true      |    false     |
  |  String   |    任何非空字符串     |      ""      |
  |  Number   | 任何非零数字值（包括无穷大） |    0和NaN     |
  |  Object   |      任何对象      |     null     |
  | Undefined |      不存在       |  undefined   |

- Number

  使用的是IEEE754格式

  1. 浮点数

  2. 数值范围

      ECMAScript的最小数值为`5e-324`，最大则是`1.79769313448623157e+308`。
      超过范围的数会被转换为`Infinity`和`-Infinity`.

      可以使用`isFinite()`函数来判断是否在非无穷

  3. NaN

    这是一个特殊的值，表示一个本来要返回数值的操作数未返回数值的情况。
    涉及到`NaN`的操作都会返回`NaN`。

    使用`isNaN()`判断其中参数是不是数值。在接到一个值后，会尝试将这个值转换为数值。某些不是数值的值会被直接转换为数值，任何不能转换为数值的值都会导致函数返回`true`。

    ````javascript
    alert(NaN == NaN); //false
    alert(0/0);   //NaN
    alert(1/0);   //Infinity
    alert(-1/0);  //-Infinity
    ````

  4. 数值转换

    数值转换函数：`Number()`、`parseInt()`、`parseFloat()`

    `Number()`可以用于任何数据类型。其转换规则如下：

    - `Boolean`会转换为1/0
    - 数字值，只是简单的传入和返回
    - `null`返回0
    - `undefined`返回`NaN`
    - 字符串如果非数值或者空，会转换为`NaN`
    - 转换对象，会先对对象使用`valueOf()`后进行转换。如果结果是`NaN`，则调用`toString()`后依照规则转换为返回的字符串值。

    转换字符串 `parseInt()`-忽略前面的空格，直至找到第一个非空格字符，如果第一个字符不是数字字符或者负号，就会返回NaN。
    ````javascript
    var num1 = parseInt（"1234blue"); //1234
    var num2 = parseInt("");          //NaN
    var num3 = parseInt("0xA");       //10
    var num4 = parseInt("22.5");      //22
    ````
    为了消除`parseInt`的一些影响，为其添加第二个参数。
    ````javascript
    var num1 = parseInt("AF",16)  //175
    var num2 = parseInt("AF");   //NaN
    var num3 = parseInt("10",2); //2（按照二进制）
    var num3 = parseInt("10",8); //8
    var num3 = parseInt("10",10); //10
    ````

- String

  `JS`里面的单双引号的作用是相同的，并且其创建的字符串是不可变的，要改变它必须先销毁原有的字符串。

  字符串转换：

  - `toString()` 
  - `+ ""`

- Object

  ```javascript
  var o = new Object();
  ```

  ​

  ### 操作符

  `--` `++`递增递减操作符

  在应用于`ture`时，将其先转换为`1`,再进行运算。`false`先转换为`0`，再运算。

  ​

  `==`操作符

  如果两个操作数相等，返回`true`，反之`false`。两个操作符在比较之时会先转换（强制转换），然后再进行比较。

- 如果有一个操作数是布尔型，则比较的相等性时先将其转换为数值false转换为0，而true转换为1。
  - 如果有一个操作数是字符串，另一个是数值，在比较时先将字符串转换为数值。
  - 如果一个操作数是对象，另一个不是，则调用对象的`valueOf()`方法，用得到的基本类型值按照前面的规则比较。
  - `null`和`undefined`是相等的。
  - 要比较相等性之前，不能将`null`和`undefined`转换为其他值。
  - 如果有一个操作数是`NaN`，则相等操作符返回`false`。
  - 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数是同一对象，则返回`true`，否则`false`。


  `===`操作符

  在未转换的情况下进行比较。

  **其余操作符均与C、java相同。**
  ### 语句

- if
  - do-while
  - while
  - for
  - for-in
  - label
  - break,continue
  - with
  - switch


  ### 函数
  ````javascript

    function functionName(arg0,arg1,arg2,...,argN){
      statements；
    }
  ````
  **没有重载**

  在ECMAScript中定义两个相同名字的函数，则该名字只属于后定义的函数。


# 基本类型

ECMAScript变量包含两种不同数据类型的的值：基本类型值和引用类型值。


ECMAScript中所有函数的参数都是按值传递的。很多同学都以为对象不是按值传递：
  ````javascript
  function setName(){
    obj.name = "Nichols";
    obj = new Object();
    obj.name = "Greg";
  }
  var person = new Object();
  setName(person);
  alert(person.name);//"Nichols"
  /*
  即使在函数内部改变参数的值，单原始的引用仍然没有变。
  实际上，当在函数内部重写obj的时候，这个变量引用的已经是一个局部对象了。这个对象会在函数执行完毕之后被销毁。
  */
  ````
### 引用类型-类

引用类型的值（对象）是引用类型的实例。在ECMASript中，引用类型是一种数据结构，用于把数据和功能组织到一起。它也常被称作**类**。
### Object

创建Object实例有两种方法：

  ````javascript
  var person = new Object();//第一种方法
  person.name = "wangli";
  person.age = 18;

  var person = {
    name : "wangli";//第二种方法
    age : 18;
  }
  ````
### Array

创建数组也有两种方法：
  ````javascript
var colors = new Array();
var colors = new Array("red","blue","green");

var colors = ["red","blue"];
var name = [];
  ````
**数组常用方法**
- `toString`

  返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。
- `pop(),push()`
- 队列方法（先进先出）

  `shift(),unshift()`在数组第一位进行操作
- 排序

 `sort() reverse()`
- `concat(),slice(),splice()`
 ````javascript
  var colors = ["red","green","blue"];
  var color1 = colors.concat("yellow",["black","brown"]);
  var color2 = colors.slice(1);
  var color3 = color1.slice(1,3);
  alert(colors);  //red,green,blue
  alert(color1);  //red,green,blue,yellow,black,brown
  alert(color2);  //green,blue
  alert(color3);  //grenn,blue,yellow

  var colors = ["red","green","blue"];
  var removed = color.splice(0,1);//删除第一项
  alert(colors);  //green,blue
  alert(removed); //red
  removed = colors.splice(1,0,"yellow","orange");//从位置1处插入两项
  alert(colors);//green,yellow,orange,blue
  alert(colors);//返回一个空数组
  removed = colors.splice(1,1,"red","purple")//插入两项，删除一项
  alert(colors); //green,red,purple,orange,blue
  alert(removed);//yellow,返回的数组中只包含一项

 ````
- 位置方法

  `indexOf()`从数组开头位置查找
  `lastIndexOf()`从数组末尾开始向前查找
- 迭代方法

  `every`对数组的每一项运行指定函数，如果每项都返回true，则返回true。

  `filter()`对数组中每个返回true的项组成数组并返回。

  `map(),some()`都是用来查询数组中的项是否满足某个条件。对于every，传入的函数必须对每一项返回true,才会返回true，否则是false。而some（）则是只要有一项返回true，则返回true。
- 归并方法
  `reduce(),reduceRight()`
````javascript
var values = [1,2,3,4,5];
var sum = values.reduce(function(prex,cur,index,array){
  return prev+cur;
})  
alert(sum); //15
````

### Date类型

### RegExp 类型

RegExp的每个实例都有下列属性：

- global: 布尔值，表示是否设置g标志。
- ignoreCase： 布尔值，表示是否设置i标志。
- lastIndex： 整数，表示开始搜索下一个匹配项的字符位置，从0开始。
- multiline： 布尔值，表示是否设置m标志。
- source： 正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。

### Function 类型

函数声明和函数表达式：
在解析器中，会先读取函数声明，并使其在执行任何代码前可用；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被执行。
````javascript
alert(sum(10,10));//正常执行
function sum(sum1,sum2){
  return sum1+sum2;
}

alert(sum(10,10)); //产生错误
var sum = function(num1, num2){
  return sum1+sum2;
};
````
函数内部属性：

- **this**

  对于javaScript，万物皆对象，而`this`就执行着当前对象
  ````javascript
  function f(){
    return "天气："+this.weather;
  }
  var sunDay{
    palyer : liming;
    weather : hot;
    hh : f
  }
  var monDay{
    palyer : lihua;
    weather : cool;
    hh :f
  }
  alert(sunDay.hh);//天气：hot
  alert(monDay.hh);//天气：cool
  ````
  `this`是非常灵活的，但是有时我们也需要将其固定下来。`javascript`提供了三个方法`apply bind call`来固定\切换`this`的指向
  - `call`

  指定`this`的指向，然后在所指定的作用域，调用其函数。
  ````javascript
  function sum(sum1,sum2){
    return sum1+sum2;
  }
   function callSum(num1,num2){
     return sum.call(this, num1, num2);
   }
   alter(callSum(10,10));  //20
  ````
  - `apply`

  `apply`方法与`call`相似，只不过传递的是数组。
  ````javascript
  function sum(num1, num2){
    return num1+num2;
  }
  function callSum1(num1,num2){
    return sum.apply(this ,arguments);
  }
  function callSum2(num1, num2){
    return sum.apply(this , [num1,num2]);
  }
  alter(callSum1(10,10)); //20
  alter(callSum2(10,10)); //20
  ````
  - `bind`

  `bind`方法用于将函数体内的`this`绑定到某个对象，然后返回一个新函数。
  ````javascript
  var add = function (x, y) {
    return x * this.m + y * this.n;
  }

  var obj = {
    m: 2,
    n: 2
  };

  var newAdd = add.bind(obj, 5);

  newAdd(5)
  // 20
  ````

### 基本包装类型

从代码来看
````javascript
var s1 = "some text";
var s2 = s1.subString(2);
​`````
在执行上述代码时后台操作是：
1. 创建String类型的一个实例。
2. 在实例上调用指定的方法。
3. 销毁这个实例。
操作如同下面的代码：
​````javascript
var s1 = new String("some text");
var s2 = s1.subString(2);
s1 = null;
````
引用类型与基本包装类型的区别就是对象的生存期。使用`new`操作符创建的引用类型的实例，在执行流离开当前作用域之前一直保存在内存中。而
自动创建的基本包装类型对象，则只存在于一行代码的瞬间执行，然后销毁。这意味着我们并不能在运行时为基本类型添加属性和方法。


`Boolean`类型

  不建议使用

`Number`类型

  要创建Number对象，可以调用Number构造函数时向其中传递相应的数值
`var numberObject = new Number(10);`

`String`类型

  `String`类型是字符串的对象包装类型，可以像下面这样使用`String`构造函数来创建
`var stringObject = new String("hello world");`

1. 字符方法

  `charAt() charCodeAt()`,例如：
````javascript
var stringValue = "hello world";
alert(stringValue.charAt(1)); //"e" ,输出的是字符
alert(stringValue.charCodeAt(1)); //"101" 输出的是字符编码（utf-8）
alert(stringValue[1]); //"e"
````

2. 字符串操作方法

  `conact`字符串拼接

  `slice、substr、substring`三种创新字符串的方法
````javascript
var stringValue = "hello world";
alert(stringValue.slice(-3));//"rld"
alert(stringValue.substring(-3);//"hello world"  负值会被转化成0
alert(stringValue.substr(-3));//"rld"
alert(stringValue.slice(3,-4));//"lo w"
alert(stringValue.substring(3,-4));//"hel"
alert(stringValue.substr(3,-4));//""
````
3. 字符串位置方法

  `indexOf、lastIndexOf`

4. `trim`

  删除字符串前后的空格

5. 字符串大小写转换方法
6. 字符串模式匹配
7. `localeCompare`

  ````javascript
  var stringValue = "yellow";

  alert(stringValue.localeCompare("brick"));  //1
  alert(stringValue.localeCompare("yellow")); //0
  alert(stringValue.localeCompare("zoo"));    //-1
  ````
8. `fromCharCode`

  将字符编码转换为字符串
````JavaScript
alert(String.fromCharCode(104, 101, 108, 108,111)); //"hello"
````
9. `eval`
````JavaScript
eval("alert('hi')");
alert('hi');  //两句完全相同
````
10. random
````javascript
值 = Math.floor(Math.random()*可能值的总数+第一个可能的值)
````
