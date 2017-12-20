# IO

## IO类

| Head File | Type                                     |
| --------- | ---------------------------------------- |
| iostream  | istream, wistream 从流读取数据                 |
|           | ostream, wostream 向流写入数据                 |
|           | iostream, wiostream 读写流                  |
| fstream   | ifstream, wifstream  从文件读取数据             |
|           | ofstream, wofstream 从文件写入数据              |
|           | fstream, wfstream 读写文件                   |
| sstream   | istringstream, wistringstream 从string读取数据 |
|           | ostringstream, wostringstream 向string写入数据 |
|           | stringstream, wstringstream 读写string     |

### IO对象无拷贝或赋值

## 文件输入输出

**fstream特有操作**

|                        |                                          |
| ---------------------- | ---------------------------------------- |
| fstream fstrm          | 创建一个未绑定的文件流。                             |
| fstream fstrm(s)       | 创建一个fstream，并打开名为s的文件。s可以是String类型，或c风格指针 |
| fstream fstrm(s, mode) | 与上一个类似，但按照指定mode打开文件                     |
|                        |                                          |
|                        |                                          |

