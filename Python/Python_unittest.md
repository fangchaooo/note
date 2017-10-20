[TOC]



# Unittest



## 四个概念

- **Test Case**

  一个完整的测试用例就是一个完整的测试流程

- **Test Suite**

  一个功能的验证往往需要多个测试用例，可以把多个测试用例集合在一起执行，这样就产生了测试套件TestSuite的概念

- **Test Runner**

  每个单元测试框架都会提供丰富的执行策略和执行结果，在unittest中，通过`TextTestRunner`类提供的run()方法来执行test suite/ test case，其可以使用图形界面、文本界面或者返回一个特殊的值等方式来表示测试执行的结果。

- **Test Fixture**

  对一个测试用例环境的搭建和销毁，就是一个fixture,通过覆盖Test Case的setUp()和tearDown()方法来实现。






## 断言方法

|            方法             |          检查          |
| :-----------------------: | :------------------: |
|     assertEqual(a,b)      |         a==b         |
|    assertNotEqual(a,b)    |         a!=b         |
|       assertTrue(x)       |   bool(x) is True    |
|      assertFalse(x)       |   bool(x) is False   |
|      assertIs(a, b)       |        a is b        |
|     assertIsNot(a,b)      |      a is not b      |
|      assertIsNone(x)      |       x is No        |
|    assertIsNotNone(x)     |    x is not None     |
|       assertIn(a,b)       |        a in b        |
|     assertNotIn(a,b)      |      a not in b      |
|  assertIsInstance(a, b)   |   isinstance(a, b)   |
| assertNotIsInstance(a, b) | not isinstance(a, b) |





## 基本代码

```python
testpro/
|------count.py
|------testadd.py
|------testsub.py
|------runtest.py

# testadd.py
from calculator import Count
import unittest

class TestAdd(unittest.TestCase):
   def setUp(self):
    ...
   def tearDown(self):
    ...
   def test_sub(self):
    ...
   def test_sub2(self):
   ...
   
# runtest.py
import unittest

test_dir = './'
discover = unittest.defaultTestLoader.discover(test_dir, pattern='test*.py')

if __name__ == '__main__':
	runner = unittest.TextTestRunner()
	runner.run(discover)
	
	
```



## 用例执行顺序

unittest是根据ASCII码的顺序加载测试用例的。



## 跳过测试和预期失败

`unittest.skip(reason)`

无条件的跳过装饰的测试，说明跳过测试的原因

`unittest.skipIf(condition, reason)`

跳过装饰的测试，如果条件为真

`unittest.skipUnless(condition, reason)`

跳过装饰的测试，除非条件为真

`unittest.expectedFailure()`

测试标记为失败，不管执行结果是否失败，统一标记为失败



