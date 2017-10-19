# 使用`CSS`

 `<style type="text/css">`

 `<link href="" type="text/css rel="stylesheet />`





# `CSS`选择器

 `* {}` 通用选择器

 `h1,h2,h3 {}` 类型选择器

`.note {}` 类选择器

  `p.note{}` 只应用`class`为`note`的`p`元素

 `#xx {}` `ID`选择器

 `li>a {}`子元素选择器，匹配指定元素的直接子元素

 `p a {}`后代选择器，匹配指定元素的所包含的后代元素

 `h1+p {}`相邻兄弟选择器

 `h1~p {}` 普通兄弟选择器





# `CSS`特性选择器

`[]` 匹配一种特定的特性

  - `p[class]`匹配所有包含`class`特性的`p`元素

`[=]`精确选择器

  - `p[class="dog"]`匹配所有`class`特性值为`dog`的`p`元素

`[~=]`部分选择器

  - `p[class~="dog"]`匹配特定的`p`中`class`有`dog`的元素

`[^=]`开头选择器

  - `p[attr^"d"]`匹配特定的`p`，这个元素的某个特性值以字母`d`开头

`[*=]`包含选择器

  - `p[attr*"do"]`匹配特定的`p`，这个元素的某个特性值中含有`do`

`[$=]`结尾选择器

  - `p[attr$"g"]`匹配特定的`p`，这个元素的某个特性值以字母`g`结尾






# `CSS`规则

- 行内>内部>外部

  > link链入外部样式和style内部样式优先级，取决于先后顺序。

- 样式表中优先级

   > ID选择器>class选择器>标签选择器>通配符

就近原则

 具体性原则

 重要性原则

  - 可在任何属性后面加 `!important`,使这条属性更重要。

继承

  - 可以从父元素继承大多数属性，用`inherit`来强制继承父元素属性







# 颜色

 `color`前景色

 `background-color`背景色

 `opacity：y`透明度

`rgba(x,x,x,y)`透明度为`y%(0-1.0)`

- `hsl(色调，饱和度，明度)`

- `hsla（色调，饱和度，明度,透明度）`

  - 色调：0-360
  - 饱和度：百分数
  - 明度：百分数（0黑色 50正常 100白色）
  - 透明度：0-1.0






# 背景

背景包括边框

`background-color`



`background-image`

默认的，背景图像位于元素的左上角，并在水平和垂直方向上重复。

- `background-repeat: repeat|no-repeat|repeat-x|repeat-y`  



`background-attachment`背景图片显示方式

- `scroll`随滚动条滚动
- `fixed`一直显示，不动



`background-position`

`background`











# 文本

- 字体

  `font-family`

- 文字大小

  `font-size`（绝对单位|相对单位）

  **绝对单位**

|   属性   |       说明       |
| :----: | :------------: |
|   in   | 1 in = 2.45cm  |
|   cm   | 1 cm = 0.394in |
|   mm   |                |
|   pt   |  72 pt = 1in   |
|   pc   |   1pc = 12pt   |
| small  |      13px      |
| medium |      16px      |
| large  |      19px      |

​	**相对单位**

​	`px`受显示器分辨率影响

​	`em/%`针对父元素进行大小设计

  - 选用更多字体

    ```css
    @font-face{
    font-family:  'xxx';
    src: url('');
    }
    ```

- 文字颜色


  `font-color`

- 文字粗细

  `font-weight:bold/normal`粗体

- 文字样式

  `font-style:normal/italic/oblique`斜体/倾斜

  - 大写和小写

    `text-transform:uppercase/lowercase/capitalize`大/小/首字母大

  - 下划线和删除线

    `text-decoration:none/undeline/overline/line-through`

  - 行间距 `line-weight`

  - 字母间距`letter-spacing`

  - 单词间距`word-spacing`

  - 对齐方式
    - `text-align:left/right/center/justify`justify表明文本两端对齐，要在宽度上占满容器。

      > text-align 只对块级元素起作用

    - `vertical-align：baseline|sub|super|top|text-top|middle|bottom|text-bottom `垂直对齐

    - 单行文字垂直居中，将`line-height`值和高度设置为相同

  - 文本缩进`text-index`

- 投影`text-shadow: x1px x2px x3px #0x0x0x`

  - `x1`阴影向左或右延伸距离(左+右-)

  - `x2`阴影向上或下延伸距离

  - `x3`可选，模糊程度

  - `#0x0x0x`投影颜色值

- 首字母和首行文本

  - `:first-line`

  - `:first-letter`

- 链接样式

  - `:link`未访问的链接

  - `:visited`单击过的链接

- 响应用户

  - `:hover`悬停

  - `:active`进行操作

  - `:focus`在元素拥有焦点





# 盒子

大小

内容溢出
`overflow`
- `hidden` 隐藏

- `scroll` 添加滚动条

边框

`border: width style color`

外边距

内边距

内联元素与块级元素的转换`display`

- `inline`使一个块级元素表现的像内联元素
- `block`使内联元素变成块级元素
- `inline-block`使块级元素像内联元素一样可以浮动并保持其他块级元素特性

盒子的隐藏`visibility`

- `hidden`隐藏
- `visible`显示





# 列表、表格和表单

列表`list-style: type image position`

项目符号样式`list-style-type`

- `none`
- `disc`
- `circle`
- `square`  

项目图像`list-style-image`

项目标记的定位`list-style-position`

- `outside`文本位于左侧（默认）
- `inside`文本缩进


空单元格的边框`empty-cells`

- `show`
- `hide`
- `inherit`继承外部表格的规则








# 布局

`position`定位

- `static`
- `relative`
- `absoulte`
- `fixed`







`float`浮动

元素设置浮动之后元素就会脱离正常文本流

- `left`
- `right`
- `inherit`跟随父元素的浮动属性
- `none`




`clear`清除浮动

- `left`
- `right`
- `both`
- `none`
