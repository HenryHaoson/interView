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

### 事件分发种onTouch和onTouchEvent的区别

onTouchEvent

![onTouchEvent1](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/onTouchEvent1.png)

![onTouchEvent2](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview1/onTouchEvent2.png)


