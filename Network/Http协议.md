# Http协议

    http协议(超文本传输协议HyperText Transfer Protocol)，
    它是基于TCP协议的应用层传输协议，
    简单来说就是客户端和服务端进行数据传输的一种规则。

默认端口：80
版本：

    HTTP/1.0，于 1996 年 5 月发布，引入了多种功能，至今仍在使用当中。
    HTTP/1.1，于 1997 年 1 月发布，持久连接被默认采用，是目前最流行的版本。
    HTTP/2 ，于 2015 年 5 月发布，引入了服务器推送等多种功能，是目前最新的版本。

## 主要特点
1. 支持客户／服务器模式
2. 简单快速：客户向服务端请求服务时，只需传送请求方式和路径。
3. 灵活：允许传输任意类型的数据对象。由Content-Type加以标记。
4. 无连接：每次响应一个请求，响应完成以后就断开连接。
5. 无状态：服务器不保存浏览器的任何信息。每次提交的请求之间没有关联。

### 非持续性和持续性

    HTTP1.0默认非持续性；HTTP1.1默认持续性

    持续性: 浏览器和服务器建立TCP连接后，可以请求多个对象
    
    非持续性: 浏览器和服务器建立TCP连接后，只能请求一个对象

### 非流水线和流水线
类似于组成里面的流水操作

    流水线：不必等到收到服务器的回应就发送下一个报文。
    
    非流水线：发出一个报文，等到响应，再发下一个报文。类似TCP。

### Http请求

<img src="../img/http请求数据结构.png" />

    http请求由请求行，消息报头，请求正文三部分构成

* 请求行：由请求方法、请求地址和 HTTP 协议版本三部分构成

例如：
>GET /example.html HTTP/1.1 (CRLF)

http协议的方法有

    GET 请求获取Request-URI所标识的资源
    POST 在Request-URI所标识的资源后附加新的数据
    HEAD 请求获取由Request-URI所标识的资源的响应消息报头
    PUT 请求服务器存储一个资源，并用Request-URI作为其标识
    DELETE 请求服务器删除Request-URI所标识的资源
    TRACE 请求服务器回送收到的请求信息，主要用于测试或诊断
    CONNECT 保留将来使用
    OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求

* 消息报头：包含一系列的键值对，允许客户端向服务器端发送一些附加信息或者客户端自身的信息，主要包括
    
Accept
Accept请求报头域用于指定客户端接受哪些类型的信息。
eg：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；Accept：text/html，表明客户端希望接受html文本。

Accept-Charset
Accept-Charset请求报头域用于指定客户端接受的字符集。
eg：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。

Accept-Encoding
Accept-Encoding请求报头域类似于Accept，但是它是用于指定可接受的内容编码。
eg：Accept-Encoding:gzip.deflate.如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。

Accept-Language
Accept-Language请求报头域类似于Accept，但是它是用于指定一种自然语言。
eg：Accept-Language:zh-cn.如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。

Authorization
Authorization请求报头域主要用于证明客户端有权查看某个资源。
当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），
可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。

Host（发送请求时，该报头域是必需的）
Host请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的

User-Agent
我们上网登陆论坛的时候，往往会看到一些欢迎信息，其中列出了你的操作系统的名称和版本，
你所使用的浏览器的名称和版本，这往往让很多人感到很神奇，
实际上，服务器应用程序就是从User-Agent这个请求报头域中获取到这些信息。
User-Agent请求报头域允许客户端将它的操作系统、浏览器和其它属性告诉服务器。
不过，这个报头域不是必需的，如果我们自己编写一个浏览器，不使用User-Agent请求报头域，
那么服务器端就无法得知我们的信息了。

* 请求正文（可选）：注意和消息报头之间有一个空行

只有在发送post请求时才会有请求正文，get方法并没有请求正文

```
GET / HTTP/1.1
Host: httpbin.org
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6,zh-TW;q=0.4
Cookie: _ga=GA1.2.475070272.1480418329; _gat=1
```
上面的第一行就是一个请求行：
GET / HTTP/1.1
其中，GET 是请求方法，表示从服务器获取资源；/ 是一个请求地址；HTTP/1.1 表明 HTTP 的版本是 1.1。

请求行后面的一系列键值对就是消息报头：

Host 是请求报头域，用于指定被请求资源的 Internet 主机和端口号，它通常从 HTTP URL 中提取出来；

Connection 表示连接状态，keep-alive 表示该连接是持久连接（persistent connection），
即 TCP 连接默认不关闭，可以被多个请求复用，如果客户端和服务器发现对方有一段时间没有活动，
就可以主动关闭连接；

Cache-Control 用于指定缓存指令，它的值有 no-cache, no-store, max-age 等，
max-age=秒表示资源在本地缓存多少秒；

User-Agent 用于标识请求者的一些信息，比如浏览器类型和版本，操作系统等；

Accept 用于指定客户端希望接受哪些类型的信息，比如 text/html, image/gif 等；

Accept-Encoding 用于指定可接受的内容编码；

Accept-Language 用于指定可接受的自然语言；

Cookie 用于维护状态，可做用户认证，服务器检验等，它是浏览器储存在用户电脑上的文本片段；

### Http响应

<img src="../img/http响应数据结构.png" />
    
    http响应也由三部分组成，包括状态行，消息报头，响应正文

* 状态行

状态行也由三部分组成，包括HTTP协议的版本，请求结果即状态码，还有对状态码的文本描述。
例如：
>HTTP/1.1 200 OK （CRLF）
    
状态码

    状态代码有三位数字组成，第一个数字定义了响应的类别，且有五种可能取值：
    1xx：指示信息--表示请求已接收，继续处理
    2xx：成功--表示请求已被成功接收、理解、接受
    3xx：重定向--要完成请求必须进行更进一步的操作
    4xx：客户端错误--请求有语法错误或请求无法实现
    5xx：服务器端错误--服务器未能实现合法的请求
    常见状态代码、状态描述、说明：
    200 OK //客户端请求成功
    400 Bad Request //客户端请求有语法错误，不能被服务器所理解
    401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate报 //头域一起使用
    403 Forbidden //服务器收到请求，但是拒绝提供服务
    404 Not Found //请求资源不存在，eg：输入了错误的URL
    500 Internal Server Error //服务器发生不可预期的错误
    503 Server Unavailable //服务器当前不能处理客户端的请求，一段时间后,可能恢复正常

* 消息报头

* 响应正文
所谓响应正文，就是服务器返回的资源的内容。即整个HTML文件。

### POST和GET的区别
| Post一般用于更新或者添加资源信息 | Get一般用于查询操作，而且应该是安全和幂等的|
| ------------- |:-------------:|
| Post更加安全 | Get会把请求的信息放到URL的后面 |
| Post传输量一般无大小限制 | Get不能大于2KB |
| Post执行效率低 | Get执行效率略高 |

### 为什么POST效率低，Get效率高

    Get将参数拼成URL,放到header消息头里传递
    Post直接以键值对的形式放到消息体中传递。
    但两者的效率差距很小很小

### Https

    端口号是443
    是由SSL+Http协议构建的可进行加密传输、身份认证的网络协议。