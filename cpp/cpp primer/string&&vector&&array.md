# 字符串、向量和数组

## 命名空间

每个名字都需要独立的using声明

头文件不应包含using声明

## String

### 定义和初始化

```c++
string s1;                     //默认初始化，s1是一个空串
string s2(s1);
string s2 = s1;
string s3("Hello");
string s3 = "value";
sring s4(n, 'c');
```

- 直接初始化
- 拷贝初始化(using '=')

### String对象操作

```c++
os<<s                
is>>s             //自动忽略开头空白，并从第一个真正的字符开始读起，直到遇到下一个空白
getline(is, s)
s.empty           //返回bool
s.size()
s[n]
s1+s2
s1==s2
s1==s2
<, <=, >, >=   //先判断相应位置上字符，然后判断长短
```

### 处理每个位置的字符-----\<cctype\>

```c++
isalnum(c)        //c is alphabet/num
isalpha(c)        //c is alphabet
iscntrl(n)        //c is control char
isdigit(c)        //c is num
isgraph(c)        //c is not blackspace but it can print
islower(c)
isprint(c)
ispunct(c)        //c is punctuation
isspace(c)        //c is space
isupper(c)
isxdigit(c)       //c is hex
tolower(c)
toupper(c)        
```

### 用for处理每个字符

```c++
for(declaration : expression)
  statement
```

## vector

### 定义

```c++
vector<T> v1;          //执行默认初始化
vector<T> v2(v1);
vector<T> v2 = v1;
vector<T> v3(n, val);  //v3包含了n个重复的元素，每个元素的值都是val
vector<T> v4(n);       //v4包含了n个重复的执行了值初始化的对象
vector<T> v5{a,b,c,...};
vectot<T> v5={a,b,c...};
```

区分列表初始化还是元素数量

```c++
vector<int> v1(10);   //v1有10个元素，每个值都是0
vector<int> v2{10};   //v2有1个元素，该元素值为10
```

### 向vector添加元素

push_back

### vector操作

```c++
v.empty();
v.size();
v.push_back();
v[n];
v1 = v2;
v1 == v2;
v1 != v2;
<, <=, >, >=
```

**vector的下标运算符可用于访问已存在元素，但不能用于添加元素**

## 迭代器

### 迭代器运算符

```c++
*iter;               //返回迭代器iter所指元素的引用
iter->item;          //解引用iter并获取该元素的名为item的成员，等价为(*iter).item
++iter;
--iter;
iter1 == iter2;
iter1 != iter2
```

### 迭代器类型

iterator可读可写

const_iterator可读不可写（常量）

begin和end返回的具体类型由对象是否常量来决定，如果对象常量，则都返回const_iterator,如果不是常量，返回iterator

cbegin,cend都返回常量

### 迭代器运算

```c++
iter + n
iter - n
iter += n
iter -= n
iter1 - iter2
> >= < <=
```

## 数组

数组不允许拷贝和赋值

```c++
int a[] = {0, 1, 2};
int a2[] = a; //error
a2 = a;       //error
```

理解复杂的数组声明