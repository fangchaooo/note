# Your first web server

```python
 # 在一个文件夹下面启动Python web server
 python -m http.server 8000
```





HTTP就是客户端（client）和服务器（server）的交互

**404** is the HTTP status code for "Not Found"

# Parts of a URI

A web address is also called a **URI** for *Uniform Resource Identifier*.

Here is an example of a URI: `https://en.wikipedia.org/wiki/Fish`

This URI has three visible parts, separated by a little bit of punctuation:

- `https` is the **scheme**;
- `en.wikipedia.org` is the **hostname**;
- and `/wiki/Fish` is the **path**.

Different URIs can have different parts; we'll see more below.

The first part of a URI is the **scheme**, which tells the client how to go about accessing the resource. Some URI schemes you've seen before include **http**, **https**, and **file**. File URIs tell the client to access a file on the local filesystem. HTTP and HTTPS URIs point to resources served by a web server.

HTTP and HTTPS URIs look almost the same. The difference is that when a client goes to access a resource with an HTTPS URI, it will use an encrypted connection to do it. Encrypted Web connections were originally used to protect passwords and credit-card transactions, but today many sites use them to help protect users' privacy. We'll look more into HTTPS near the end of this course.



## Scheme

http https file

## Hostname

In an HTTP URI, the next thing that appears after the scheme is a **hostname** — something like `www.udacity.com` or `localhost`. This tells the client which server to connect to.





## Path

In an HTTP URI (and many others), the next thing that appears is the **path**, which identifies a particular resource on a server. A server can have many resources on it — such as different web pages, videos, or APIs. The path tells the server which resource the client is looking for.

 But that's just the demo server. In the real world, URI paths don't *necessarily* equate to specific filenames.



# Hostnames and ports

## Hostnames

A full HTTP or HTTPS URI includes the hostname of the web server, like `www.udacity.com` or `www.un.int` or `www.cheeseboardcollective.coop` (my favorite pizza place in the world, in Berkeley CA). A hostname in a URI can also be an IP address: for instance, if you put <http://216.58.194.174/> in your browser, you'll end up at Google.

using `host`or`nslookup` to find IP address。





## Localhost

The IPv4 address `127.0.0.1` and the IPv6 address `::1` are special addresses that mean "this computer itself" — for when a client (like your browser) is accessing a server on your own computer. The hostname `localhost` refers to these special addresses.



## Ports

但你从浏览器中连接你的demo服务器时，你给的`URL`是`http://localhost:8000/`.

这个`URL`的端口号是`8000`。但是当你登录位网站的时候你并没有发现一些网站的端口号，是因为客户端早已计算出那些 端口号从`URL Scheme`。比如，`HTTP`的端口号是`80`，`HTTPS`的是`443`,你所运行的demo不是默认端口，所以你要自己设置。



什么是端口号？

一台拥有IP地址的[主机](https://baike.baidu.com/item/%E4%B8%BB%E6%9C%BA)可以提供许多服务，比如Web服务、FTP服务、SMTP服务等，这些服务完全可以通过1个IP地址来实现。那么，[主机](https://baike.baidu.com/item/%E4%B8%BB%E6%9C%BA)是怎样区分不同的网络服务呢？显然不能只靠IP地址，因为IP 地址与网络服务的关系是一对多的关系。实际上是通过“IP地址+端口号”来区 分不同的服务的。

服务器一般都是通过知名端口号来识别的。例如，对于每个TCP/IP实现来说，[FTP服务器](https://baike.baidu.com/item/FTP%E6%9C%8D%E5%8A%A1%E5%99%A8)的TCP端口号都是21，每个Telnet服务器的TCP端口号都是23，每个TFTP(简单文件传送协议)服务器的UDP端口号都是69。任何TCP/IP实现所提供的服务都用知名的1～1023之间的端口号。这些知名端口号由Internet号分配机构来管理。

当我们说一个服务器在监听一个端口，实际上就是讲，在服务器运行时，它告诉操作系统它想从特定的端口号来连接到客户端。当一个客户端（或者浏览器）连接到这个端口发送一个请求时，这个操作系统知道转发这个请求到那个正在监听特定端口的服务器



#  HTTP GET requests

 `GET` 是模块或一个HTTP动词被使用着; 也就是说请求被建立。`GET` is the verb that clients use when they want a server to send a resource, such as a web page or image. Later, we'll see other verbs that are used when a client wants to do other things, such as submit a form or make changes to a resource.

`/readme.png` is the **path** of the resource being requested. Notice that the client does not send the whole URI of the resource here. It doesn't say `https://localhost:8000/readme.png`. It just sends the path.

Finally, `HTTP/1.1` is the **protocol** of the request. Over the years, there have been several changes to the way HTTP works. Clients have to tell servers which dialect of HTTP they're speaking. HTTP/1.1 is the most common version today.



# HTTP responses

