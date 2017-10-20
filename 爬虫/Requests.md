# Requests



## 主要方法

`requests()`

构造一个请求，是支撑以下各方法的基础

`get(url, params=None, **kwargs)`

`head(url, **kwargs)`

`post(url, data=None, json=None, **kwargs)`

`put(url, data=None, **kwargs)`

`patch(url, data=None, **kwargs)`

`delete(url, **kwargs)`



## Response 对象属性

- ststus_code

  HTTP请求返回的状态

- text

- encoding

  从HTTP header中猜测的响应内容编码方式

  如果header中不存在charset，则认为编码为ISO-8859-1

- apparent_encoding

  从内容中分析出的响应内容编码方式

- content

  HTTP响应内容的二进制形式



`raise_for_status()`



## 异常

|            异常             |           说明            |
| :-----------------------: | :---------------------: |
| requests.ConnectionError  | 网络连接错误异常，如DNS查询失败、拒绝连接等 |
|    requests.HTTPError     |        HTTP错误异常         |
|   requests.URLRequired    |         URL缺失异常         |
| requests.TooManyRedirects |    超过最大重定向次数，产生重定向异常    |
|  requests.ConnectTimeout  |       连接远程服务器超时异常       |
|     requests.Timeout      |     请求URL超时，产生超时异常      |





## HTTP

| 方法     | 说明                             |
| ------ | ------------------------------ |
| GET    | 请求获取URL位置的资源                   |
| HEAD   | 请求获取URL位置资源的响应消息报告，即获得该资源的头部信息 |
| POST   | 请求向URL位置的资源后附加新的数据             |
| PUT    | 请求向URL位置存储一个资源，覆盖原URL位置的资源     |
| PATCH  | 请求局部更新URL位置的资源，即改变该处资源的部分内容    |
| DELETE | 请求删除URL位置存储的资源                 |



### PATCH/PUT

假设URL位置有一组数据UserInfo，包括UserID、UserName等20个字段
需求：用户修改了UserName，其他不变

- 采用PATCH，仅向URL提交UserName的局部更新请求
- 采用PUT，必须将所有20个字段一并提交到URL，未提交字段被删除

PATCH的最主要好处：节省网络带宽

