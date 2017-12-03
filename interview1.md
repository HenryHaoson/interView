### 什么事ANR，如何避免ANR

ANR 就是Android Not Responding，也就是程序无响应。

出现的场景：

- 按键或者触摸事件5秒内没有响应。
- BroadcastReceiver 10秒内无法完。
- Service 在20秒内无法处理完。

如何避免：

- UI线程避免耗时操作
- 不要滥用Android生命周期，避免在onCreate和onResume中做太多工作
- 检验内存问题
- 避免加载大图片是内存不足而ANR
- 避免内存泄露


### ddms 和traceView

ddms是Dalvik Debug Monitor Server，也就是AS中Monitor的前身。

TraceView是AndroidSDK中内置的一个工具。可以加载trace文件，用图形的方式表示代码的执行时间、次数及调用栈。

trace文件有3种生成方式

- 使用代码生成
  > 使用代码生成可以很方便的控制开始和结束的地方

    ```java

	//开始trace，文件保存到“/sdcard/tracing.trace"
	Debug.startMethodTracing("tracing");
	//...
	Debug.stopMethodTracing();

    ```

- 使用AS(moitor)
- 使用DDMS(DDMS会打开trace文件，启动traceView)

### 事件分发中onTouch和onTouchEvent的区别

onTouchEvent

当有一个屏幕触摸事件会触发这个方法（是在事件分发之后）。

![onTouchEvent1](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/onTouchEvent1.png)

可以实现这个方法处理触摸事件。如果这个方法用来监听
![onTouchEvent2](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/onTouchEvent2.png)


onTouch

这个方法是事件分发之前执行的，可以在事件传到下一个view之前处理这个Event。这也是我觉得和onTouchEvent方法区别最大的地方。

![onTouch](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/onTouch1.png)


### SSL握手过程（参考的阮同学的文章）

http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html

握手过程就是在客户端和服务端进行加密通信之前进行连接和交换参数的过程。

握手过程分为5步

- 第一步:客户端向服务端发送协议版本号，一个随机数和支持的加密算法。

- 第二步:服务端向客户端发送一个随机数和数字证书。

- 第三步:客户端确认数字证书有效，生成一个随机数，并且用数字证书中的公钥来加密这个随机数发送给服务端。

- 第四步:服务端用自己的私钥解密这个随机数。

- 第五步:客户端和服务端用上面的这3个随机数和加密算法生成一个用来对话的key（对话密钥）。

![SSL握手过程](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/ssl.png)

SLL中断Session恢复的方法。

有两种方法，一种是保存一个SessionID，服务端验证SessionID对应的Session是否存在就行。缺点是SessionID可能只存在一个台服务器上。所有浏览器都支持这种方法。

还有一种方法是SessionTicket。客户端向服务器发送服务器在上一次对话中发送过来的Session Ticket。然后就可以继续使用Session Ticket中的密钥进行对话了。

### 如何保证一个后台服务不被杀死，比较省电的方式是？

- 方法1:提升Service的进程优先级（前台Service）；

- 方法2:onStartCommand方法，返回START_STICKY；

- 方法3:在onDestroy中启动Servic（强制停止Service的时候，可能不走onDestroy方法）；

- 方法4:设置Persistent属性；

- 方法5:通过监听系统广播，来检测service是否还存活，如果死掉了就唤醒；

这只是针对Service的，进程保活文章链接http://www.jianshu.com/p/63aafe3c12af


### 序列化的几种方式(区别）

[Serializable和Paracable的区别](https://henryhaoson.github.io/2017/07/05/android%E5%BA%8F%E5%88%97%E5%8C%96%E9%97%AE%E9%A2%98/)

- Serializable

- Parcable

- Json


### view事件分发

一张图的事情，核心思想：责任链模式

![dispatch](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/dispatch1.png)


