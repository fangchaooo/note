# Beautiful Soup

**Beautiful Soup库是解析、遍历、维护“标签树”的功能库**







## 引用

```python
from bs4 import BeautifulSoup
import bs4
```



## 库解析器

`soup = BeautifulSoup('<html>data</html>', 'html.parser')`

| 解析器          | 使用方法         | 条件                   |
| ------------ | ------------ | -------------------- |
| bs4的HTML解析器  | 'html.parse' | bs4                  |
| lxml的HTML解析器 | 'lxml'       | pip install lxml     |
| lxml的XML解析器  | 'xml'        | pip install lxml     |
| html5lib解析器  | 'html5llib'  | pip install html5lib |



## Beautiful Soup类的基本元素

| 基本元素            | 说明                                     |
| --------------- | -------------------------------------- |
| Tag             | 标签，最基本的信息单元，分别用<>和</>标明开头和结尾           |
| Name            | 标签的名字,<p>...</p>的名字是'p'，格式为<tag>.name  |
| Attributes      | 标签的属性，字典形式组织，格式:<tag>.attrs            |
| NavigableString | 标签内非属性字符串,<>...</>中字符串，格式：<tag>.string |
| Comment         | 标签内字符串的注释部分，一种特殊的Commet类型              |



## 标签树的下行遍历

| 属性           | 说明                                  |
| ------------ | ----------------------------------- |
| .contents    | 子节点的列表，将<tag>所有儿子节点存入列表             |
| .children    | 孙子节点的迭代类型，与`.contents`类似，用于循环遍历儿子节点 |
| .descendants | 子孙节点的迭代类型，包含所有子孙节点，用于循环遍历           |



## 标签树的上行遍历

| 属性       | 说明                     |
| -------- | ---------------------- |
| .parent  | 节点的父亲标签                |
| .parents | 节点先辈标签的迭代类型，用于循环遍历先辈节点 |



## 标签树的平行遍历

| 属性                 | 说明                           |
| ------------------ | ---------------------------- |
| .next_sibling      | 返回按照HTML文本顺序的下一个平行节点标签       |
| .previous_sibling  | 返回按照HTML文本顺序的上一个平行节点标签       |
| .next_siblings     | 迭代类型，返回按照HTML文本顺序的后续所有平行节点标签 |
| .previous_siblings | 迭代类型，返回按照HTML文本顺序的前续所有平行节点标签 |



## prettify()方法

`.prettify()`为HTML文本<>及其内容增加跟加`\n`

`.prettify()`可用于标签，方法 : ` <tag>.prettify()`