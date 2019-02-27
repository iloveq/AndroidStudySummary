android 学习总结

### 1 内存泄露的场景有哪些？内存分析工具的使用方法？
- 单例模式引起的内存泄露
- 静态变量导致的内存泄露
- 非静态内部类引起的内存泄露
- 使用资源时，未及时关闭引起泄露
- webview导致的内存泄露


对于内存泄露的检测，常用工具有LeakCanary、MAT、AS自带的Profiler。

### 2 启动优化，有什么工具可以使用？

重点就是systrace工具的使用。


### 3 Handler,looper,messagequeue,thread,message,每个类功能,关系？

背景：从多任务执行机制上来说，有3种方式(排队，多线程，fork进程)，Handler 是第一种，著名的使用案例有 IOS-runLooper 
NodeJs-eventLooper 排队执行任务 (message) 就有对应的 messagequeue 队列，负责 存 enqueueMessage() 取 next().对于android系统来说，我们知道整个消息机制用的是 Handler，点击图标进程被fork起来，AMS会通过 startProcessLocked 函数通过 socket 连接 zygot 调用 RuntimetInit.zygotInit -> invokeStaticMain 反射调用android.app.ActivityThread 的 [main](https://github.com/woaigmz/AndroidStudySummary/blob/master/Android/main.png) 方法，初始化 Looper，attach 时候通过 Handler+AMS 的代理初始化 application. 既然我们的app可以一直正常工作，那么就需要这样一个角色 Looper.loop() 一个for循环 当然节省资源来说，没有任务处理时就会通过 MessageQueue 的 mPtr 管道，去使用linux 的 expoll 去阻塞，有任务时再去 nativeWake

-------> Handler 源码分析 👇
