# Python奇技淫巧

## 数据结构与算法

#### 多个解压

```python
x, y, z = a
x, _, z = a
x, *y, z = a
```

#### 筛选数据

- 列表、字典、集合
  - `filter------ filter(lambda x: x>=0, data)`  
  - 列表解析

#### 为元组每个元素命名

使用`collection.namedtuple`替代`tuple`

#### 计算词频

`collections.Counter`对象中`most_common(n)`方法

#### 字典项排序

1.  利用`zip`将字典中的数据转化为tuple
2. 传递sorted函数的key参数

#### 找到字典中的公共key

1. 使用字典的viewkeys()方法，得到一个字典keys的集合
2. 使用map得到所有keys集合
3. 使用reduce函数，取所有字典的keys的集合的交集

#### 使字典有序

使用`collections.OrderedDict`

#### 历史记录功能

使用`collections`中的`deque`方法，实现双端队列，在程序退出前，使用pickle将对象存入到文件





