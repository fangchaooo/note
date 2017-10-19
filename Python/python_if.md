# if

`if` statement

```python
def if_function(condition, true_result, false_result):
    if condition:
        return true_result
    else:
        return false_result
```
`if` function

```python
def with_if_statement():
    if c():
        return t()
    else:
        return f()

def with_if_function():
    return if_function(c(), t(), f())

def c():
    return False

def t():
    1/0

def f():
    return 1



    >>> with_if_statement()
    1
    >>> with_if_function()
    Traceback (most recent call last):
      File "<pyshell#1>", line 1, in <module>
        with_if_function()
      File "C:/Users/fc/Desktop/59565.py", line 13, in with_if_function
        return if_function(c(), t(), f())
      File "C:/Users/fc/Desktop/59565.py", line 19, in t
        1/0
    ZeroDivisionError: division by zero
```

在执行`with_if_statement()`时，实际上是执行标准的`if`语句，在判断为`False`后不会执行`t()`。

但是在执行`with_if_function()`时，函数返回时会依次计算`c() t() f()`，因此会出现错误。假如没有错误，则将结果带入`if_function()`，最后再回到`with_if_function()`进行返回。
