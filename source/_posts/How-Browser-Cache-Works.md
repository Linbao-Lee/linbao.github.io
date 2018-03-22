title: How Browser Cache Works
date: 2018-03-22 23:39:31
alias:
tags: 笔记
categories: 总结与分享
---
把Last-Modified和ETags请求的http报头一起使用，这样可利用客户端（例如浏览器）的缓存。因为服务器首先产生Last-Modified/Etag标记，服务器可在稍后使用它来判断页面是否已经被修改。本质上，客户端通过将该记号传回服务器要求服务器验证其（客户端）缓存

# 原理
客户端请求一个页面（A）。 服务器返回页面A，并在给A加上一个ETag。 客户端展现该页面，并将页面连同ETag一起缓存。 客户再次请求页面A，并将上次请求时服务器返回的ETag一起传递给服务器。 服务器检查该ETag，并判断出该页面自上次客户端请求之后还未被修改，直接返回响应304（未修改——Not Modified）和一个空的响应体

# Etag
Etag 是URL的Entity Tag，用于标示URL对象是否改变

# Last-Modified
在浏览器第一次请求某一个URL时，服务器端的返回状态会是200，内容是客户端请求的资源，同时有一个Last-Modified的属性标记此文件在服务器端最后被修改的时间。
客户端第二次请求此URL时，根据HTTP协议的规定，浏览器会向服务器传送If-Modified-Since报头，询问该时间之后文件是否有被修改过
如果服务器端的资源没有变化，则自动返回 HTTP 304（Not Changed.）状态码，内容为空，这样就节省了传输数据量。当服务器端代码发生改变或者重启服务器时，则重新发出资源，返回和第一次请求时类似。从而保证不向客户端重复发出资源，也保证当服务器有变化时，客户端能够得到最新的资源

# HTTP中的缓存机制

网页的缓存是由HTTP消息头中的“Cache-control”来控制的，常见的取值有private、no-cache、max-age、must- revalidate等，默认为private。其作用根据不同的重新浏览方式分为以下几种情况：

### （1） 打开新窗口
值为private、no-cache、must-revalidate，那么打开新窗口访问时都会重新访问服务器。
而如果指定了max-age值，那么在此值内的时间里就不会重新访问服务器，例如：
Cache-control: max-age=5(表示当访问此网页后的5 秒 内再次访问不会去服务器)

### （2） 在地址栏回车
值为private或must-revalidate则只有第一次访问时会访问服务器，以后就不再访问。
值为no-cache，那么每次都会访问。
值为max-age，则在过期之前不会重复访问。

### （3） 按后退按扭
值为private、must-revalidate、max-age，则不会重访问，
值为no-cache，则每次都重复访问

### （4） 按刷新按扭
　 无论为何值，都会重复访问

Cache-control值为“no-cache”时，访问此页面不会在Internet临时文章夹留下页面备份。

# 禁止页面在IE中缓存 

```
CacheControl = no-cache
Pragma=no-cache
Expires = -1 /*Expires 表示存在时间，允许客户端在这个时间之前不去检查（发请求），等同max-age的 
效果。但是如果同时存在，则被Cache-Control的max-age覆盖。 */
```

# HTTP 状态码

目录

#### 1 消息（1字头）
- 100 Continue
- 101 Switching Protocols
- 102 Processing

#### 2 成功（2字头）
 - 200 OK
 - 201 Created
 - 202 Accepted
 - 203 Non-Authoritative Information
 - 204 No Content
 - 205 Reset Content
 - 206 Partial Content
 - 207 Multi-Status

#### 3 重定向（3字头）
 - 300 Multiple Choices
 - 301 Moved Permanently
 - 302 Move temporarily
 - 303 See Other
 - 304 Not Modified
 - 305 Use Proxy
 - 306 Switch Proxy
 - 307 Temporary Redirect

#### 4 请求错误（4字头）
 - 400 Bad Request
 - 401 Unauthorized
 - 402 Payment Required
 - 403 Forbidden
 - 404 Not Found
 - 405 Method Not Allowed
 - 406 Not Acceptable
 - 407 Proxy Authentication Required
 - 408 Request Timeout
 - 409 Conflict
 - 410 Gone
 - 411 Length Required
 - 412 Precondition Failed
 - 413 Request Entity Too Large
 - 414 Request-URI Too Long
 - 415 Unsupported Media Type
 - 416 Requested Range Not Satisfiable
 - 417 Expectation Failed
 - 422 Unprocessable Entity
 - 423 Locked
 - 424 Failed Dependency
 - 425 Unordered Collection
 - 426 Upgrade Required
 - 449 Retry With
 - 451Unavailable For Legal Reasons

#### 5 服务器错误（5、6字头）
 - 500 Internal Server Error
 - 501 Not Implemented
 - 502 Bad Gateway
 - 503 Service Unavailable
 - 504 Gateway Timeout
 - 505 HTTP Version Not Supported
 - 506 Variant Also Negotiates
 - 507 Insufficient Storage
 - 509 Bandwidth Limit Exceeded
 - 510 Not Extended
 - 600 Unparseable Response Headers
