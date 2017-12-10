---

layout: post
title: Http缓存机制 
date: 2017-12-10
categories: blog
tags: [http]
description: http缓存机制以及okhttp对http缓存协议的实现 

---

## http协议中的缓存机制

在http1.0中，可以在请求头里边添加Expires字段。

设置这个字段就可以在缓存过期前读取缓存而不是向服务器发送新的请求（历史遗留产物）。

在http1.1中，又添加了新的字段cache-control字段。这个字段是用来替换Expires字段的。

cache-control 字段主要是确定采用的缓存策略。主要的缓存策略可以分为两类，一个是针对request的，一个是针对response的。

> request

```

Cache-Control: max-age=<seconds>
Cache-Control: max-stale[=<seconds>]
Cache-Control: min-fresh=<seconds>
Cache-Control: no-cache 
Cache-Control: no-store
Cache-Control: no-transform
Cache-Control: only-if-cached

```

> response

```

Cache-Control: must-revalidate
Cache-Control: no-cache
Cache-Control: no-store
Cache-Control: no-transform
Cache-Control: public
Cache-Control: private
Cache-Control: proxy-revalidate
Cache-Control: max-age=<seconds>
Cache-Control: s-maxage=<seconds>

```

还有其他的一些策略

```

Cache-Control: immutable 
Cache-Control: stale-while-revalidate=<seconds>
Cache-Control: stale-if-error=<seconds>


```

针对缓存的一些策略

- public:表明response可以被任何缓存缓存。

- private:表明只能被单一用户缓存。不能被共享的缓存区缓存。

- no-cache:禁止使用缓存，必须发送新的请求。

- only-if-cached:指检索缓存数据，不发送请求。


针对缓存时长（是否过期)的策略

- max-age=<seconds>:指定缓存可以存活的最大时长。和expires相反，他的时间是依赖request的时间都。

- s-maxage=<seconds>:覆盖max-age或者是expires字段的，只对共享的缓存区游泳，对私有缓存是没有用的。

- max-stale[=<seconds>]:指示客户机可以接收超出超时期间的响应消息。如果指定max-stale消息的值，那么客户机可以接收超出超时期指定值之内的响应消息.

- min-fresh=<seconds>:要求在指定的时间段内，请求必须是最新的。

- stale-while-revalidate=<seconds>:表明现接受过期的缓存，但是异步请求新的数据。时间是愿意接受的过期时长。


针对重新生效的策略

- must-revalidate:在使用这个之前必须确定过期资源的状态，过期的数据是不会采用的。

- proxy-revalidate:和must-revalidate一样，但是制作用在共享的缓存区上。

- immutable:304(逃

还有一些其他的策略

- no-store:缓存不应该缓存http请求或者是响应体。

- no-transform:不能够转换资源。例如Content-Encoding, Content-Range, Content-Type headers这些不能够被一些代理修改。



