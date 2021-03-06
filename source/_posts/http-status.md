---
title: HTTP状态码
date: 2017-10-12 22:39:05
tags:
---

# 访问网页的各种http状态码的解释说明

### 一、1xx信息系列
```
100：continue->服务器仅仅接受到部分请求，但是一旦服务器没有拒绝该请求，客户端应该继续
    发送其他请求
101：switching protocols->服务器转换协议，服务器将遵从客户的请求转换到另一种协议
102：processing->处理将继续执行
```
<!-- more -->
### 二、2xx信息系列
  2XX类型的状态码代表着请求已经被服务器接收、理解、并接受
```
200 OK：请求成功(其后是对GET和POST请求的应答文档。)
201 Created — 请求被创建完成，同时新的资源被创建。
202 Accepted — 供处理的请求已被接受，但是处理未完成。
203 Non-authoritative Information — 文档已经正常地返回，但一些应答头可能不正确，因为
    使用的是文档的拷贝。
204 No Content — 没有新文档。浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而
    Servlet可以确定用户文档足够新，这个状态代码是很有用的。
205 Reset Content — 没有新文档。但浏览器应该重置它所显示的内容。用来强制浏览器清除表单
    输入内容。
206 Partial Content — 客户发送了一个带有Range头的GET请求，服务器完成了它。
207 Multi-Status — 由WebDAV(RFC 2518)扩展的状态码，代表之后的消息体将是一个XML消息，
    并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。
```

### 三、3xx信息系列
  3XX这类状态码代表着客户端需要采取进一步的操作才能完成请求，通常，这些状态码是用来重定向的，
按照 HTTP/1.0 版规范的建议，浏览器不应自动访问超过5次的重定向。
```
300 Multiple Choices — 多重选择。链接列表。用户可以选择某链接到达目的地。最多允许五个地址。
301 Moved Permanently — 所请求的页面已经转移至新的url。
302 Found — 所请求的页面已经临时转移至新的url。
303 See Other — 所请求的页面可在别的url下被找到。
304 Not Modified — 未按预期修改文档。客户端有缓冲的文档并发出了一个条件性的请求(一般是提供
    If-Modified-Since头表示客户只想比指定日期更新的文档)。服务器告诉客户，原来缓冲的文档还
    可以继续使用。
305 Use Proxy — 客户请求的文档应该通过Location头所指明的代理服务器提取。
306 Unused — 此代码被用于前一版本。目前已不再使用，但是代码依然被保留。
307 Temporary Redirect — 被请求的页面已经临时移至新的url。
```
### 四、4xx信息系列
  4XX类型的状态码代表着客户端可能发生了错误，阻碍了服务器的处理，
```
400 Bad Request — 服务器未能理解请求或是请求参数有误。
401 Unauthorized — 被请求的页面需要用户名和密码。
402 Payment Required — 此代码尚无法使用(为了将来可能的需求而预留的。)
403 Forbidden — 对被请求页面的访问被禁止。
404 Not Found — 服务器无法找到被请求的页面。
405 Method Not Allowed — 请求中指定的方法不被允许。
406 Not Acceptable — 服务器生成的响应无法被客户端所接受。
407 Proxy Authentication Required — 用户必须首先使用代理服务器进行验证，这样请求才会被处理。
408 Request Timeout — 请求超出了服务器的等待时间。
409 Conflict — 由于冲突，请求无法被完成。
410 Gone — 被请求的页面不可用。
411 Length Required"Content-Length — " 未被定义。如果无此内容，服务器不会接受请求。
412 Precondition Failed — 请求中的前提条件被服务器评估为失败。
413 Request Entity Too Large — 由于所请求的实体的太大，服务器不会接受请求。
414 Request-url Too Long — 由于url太长，服务器不会接受请求。当post请求被转换为带有很长的查询
    信息的get请求时，就会发生这种情况。
415 Unsupported Media Type — 由于媒介类型不被支持，服务器不会接受请求。
416 — 服务器不能满足客户在请求中指定的Range头。
417 Expectation Failed
```

### 五、5xx信息系列
  这类状态码代表了服务器在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软
硬件资源无法完成对请求的处理。
```
500 Internal Server Error — 请求未完成。服务器遇到不可预知的情况。
501 Not Implemented — 请求未完成。服务器不支持所请求的功能。
502 Bad Gateway — 请求未完成。服务器从上游服务器收到一个无效的响应。
503 Service Unavailable — 请求未完成。服务器临时过载或当机。
504 Gateway Timeout — 网关超时。
505 HTTP Version Not Supported — 服务器不支持请求中指明的HTTP协议版本。
```
http协议响应状态码看起来很多，但若不是需要做AJAX，REST,网络爬虫，机器人等程序，我们只需要了解常见
的200、302.304.404、503这几个状态码就好了。
原文装载自:http://blog.csdn.net/u012562943/article/details/51729691