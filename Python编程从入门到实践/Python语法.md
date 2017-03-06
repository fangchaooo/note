# Python

```Python
Beautiful is better than ugly.

Explicit is better than implicit.

Simple is better than complex.

Complex is better than complicated.

Flat is better than nested.

Sparse is better than dense.

Readability counts.

Special cases aren't special enough to break the rules.

Although practicality beats purity.

Errors should never pass silently.

Unless explicitly silenced.

In the face of ambiguity, refuse the temptation to guess.

There should be one-- and preferably only one --obvious way to do it.

Although that way may not be obvious at first unless you're Dutch.

Now is better than never.

Although never is often better than *right* now.

If the implementation is hard to explain, it's a bad idea.

If the implementation is easy to explain, it may be a good idea.

Namespaces are one honking great idea -- let's do more of those!
```
### 变量和简单数据类型

变量
- 变量名只能包含字母、数字和下划线
- 变量名不能包含空格
- 不能将`Python`关键字当做变量名


字符串：用`''`或者`""`都可以包含字符串。对字符串中单词有常见的几种方法：

- `.title()` 以首字母大写显示每个单词
- `.upper()` 字符串全为大写
- `.lower()` 字符串全为小写
- `+` 合并字符串
- `\t`和`\n`制造空白
- `.strip()`删除所有空白（删除的空白并没有保存到原字符串，下同）
- `.rstrip()`删除尾部空白
- `.lstrip()`删除首部空白

浮点数（遵循`IEEE754`标准）

`str()`进行类型转换

`#`注释

### 列表

列表是由一系列按特定顺序排列的元素组成的**值**。

列表用下标取值。（从`0`开始，可取负值，从倒数开始`-1,-2`）

列表基础操作
- `+`列表连接

- 列表添加元素

  - `.append()`列表末尾添加元素
  - `.insert(index, obj)`特定位置添加元素

- 列表删除元素

  索引删除
  - `del`删除元素，需指定元素索引
  - `.pop(boj=list[-1])`

  值删除
  - `.remove()`

- 排序
  - `.sort()`对列表进行永久性排序

    `.sort(reverse=True)`逆序排序
  - `sorted()`不影响原始排列顺序的
- 列表反转`.reverse()`

遍历`for x in list`

创建数值列表`range(start, end, scan)`

列表解析
>列表解析式是将一个列表（实际上适用于任何可迭代对象（iterable））转换成另一个列表的工具。在转换过程中，可以指定元素必须符合一定的条件，才能添加至新的列表中，这样每个元素都可以按需要进行转换。

```python
for x in range(1,10):
  result = x*2
#列表解析式  
result = [x*2 for x in range(1,10)]
```
切片
```python
>>> spam = ['cat', 'bat', 'rat', 'elephant']
>>> spam[0:3]  #从索引0开始到索引2，不包含3，共3个元素
['cat', 'bat', 'rat']
>>> spam[1:3]
['bat', 'rat']
>>> spam[0:-1] #从索引0到最后一个元素，不包含最后一个索引为-1的元素
['cat', 'bat', 'rat']
>>> spam[:2] #从索引0到1
['cat', 'bat']
>>> spam[1:] #从索引1到end
['bat', 'rat', 'elephant']
>>> spam[:] #列表复制
['cat', 'bat', 'rat', 'elephant']
```
**元组** -----特殊的列表（<span style="border-bottom:3px bolid #0F0">不可变列表</span>）

修改元组只能重新赋值

### 流程

`Boolean`------`True` `False`

布尔操作符-------`and` `or` `not`

`if - else`

`if - elif - else`

`while`

`for in`

### 字典

`d = {key1: value1, key2: value2}`

字典是**不排序**的

对字典的基本操作
```python
#访问字典中的值
print(d[key1]） #value1

#添加键值
d['like'] = 'cat'

#创建空字典
mycat = {}

#修改值
d['like'] = 'dog'

#删除键-值对
del d['like']
```
字典的遍历

```python

# 遍历键
>>> a = {'like': 'cat', 'heat' : 'dog'}
>>> for k in a:
	   print(k)

heat
like

>>> for k in a.keys():
	print(k.title())
# keys() 并非只能用于遍历，实际上它返回的是一个列表，其中包含字典中的所有键

Heat
Like
# 遍历值
>>> for v in a.values():
	print(v.upper())


DOG
CAT

# 遍历键-值
>>> for k, v in a.items():
	   print(k+ ' '+ v)


heat dog
like cat
```

用`in`检查字典中是否存在键或值

`.get(s1, s2)`
- `s1`要取得的值得键
- `s2`如果键不存在返回的备用值

`.setdefault(x1, x2)`
- `x1`要检查的键
- `x2`如果该键不存在时要设置的值（如果该键确实存在，方法会返回键的值）

## 函数

`def fun_name(参数...)`

关键字实参

`def function_name(para)`
