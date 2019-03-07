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
NodeJs-eventLooper 排队执行任务 (message) 就有对应的 messagequeue 队列，负责 存 enqueueMessage() 取 next().对于android系统来说，我们知道整个消息机制用的是 Handler，点击图标进程被fork起来，AMS会通过 startProcessLocked 函数通过 socket 连接 zygot 调用 RuntimetInit.zygotInit -> invokeStaticMain 反射调用android.app.ActivityThread 的 [main](https://github.com/woaigmz/AndroidStudySummary/blob/master/Android/main.pnghttps://github.com/woaigmz/AndroidStudySummary/blob/master/Android/img/main.png) 方法，初始化 Looper -> Looper.prepareMainLooper(); 然后 ActivityThread attach 时候通过 Handler + AMS 的代理初始化 application. 既然我们的app可以一直正常工作，那么就需要这样一个角色 Looper.loop() 一个for循环 当然节省资源来说，没有任务处理时就会通过 MessageQueue 的 mPtr 管道，去使用 linux 的 epoll 去阻塞，有任务时再去 nativeWake

- Handler 这个类在 android 消息机制中起什么作用，有哪些成员变量，有哪些重要的方法
- Looper 构造函数做了什么，怎么实例化的，为什么要把Looper存在ThreadLoacl里，它又有哪些重要方法
- Message 有哪些成员变量，有几种实例化方式，obtain方法什么时候用服用sPool的内存
- MessageQueue 是谁来初始化，构造方法为什么有 native 方法，成员变量都是做什么用的，作为队列对外暴漏了哪些方法
- 作为消息机制，用了什么样的架构模式，Handler，looper，messagequeue，thread，message，这些对象有样的联系

-------> [Handler 源码分析](https://github.com/woaigmz/AndroidStudySummary/blob/master/Android/Handler%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90.md) <-------

### 4 探讨 android 线程优先级及子线程的使用？

线程优先级的设置 Process.setThreadPriority(); new Thread().setPriority();

##### 主线程：
- Activity 的所有生命周期回调都是执行在主线程的.
- Service 默认是执行在主线程的.
- BroadcastReceiver 的 onReceive 回调是执行在主线程的.
- 没有使用子线程的 looper 的 Handler 的 handleMessage, post(Runnable) 是执行在主线程的.
- AsyncTask 的回调中除了 doInBackground, 其他都是执行在主线程的.
- View 的 post(Runnable) 是执行在主线程的

##### 子线程：
- 继承 Thread
- 实现 Runnable 接口
- 使用 AsycnTask doInBackground 方法
- HandlerThread
- IntentService https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/java/android/app/IntentService.java   (使用了           HandlerThread的looper)
- Loader - CursorLoader

### 5 探讨 android 进程，进程间通信方式，多进程的应用？

### 6 探讨 android 事件分发机制

### 7 探讨 android view 绘制流程

### 8 探讨 android service

### 9 探讨 android 系统从点击图标到应用启动

### 10 探讨 android 系统从开机到 Launcher 

### 11 SharedPerference 源码分享

### 12 binder机制

### 13 contentprovider 原理及底层实现，不同 api 的 getType区别

### 14 Surface SurfaceView SurfaceHolder 

### 15 activity 生命周期及管理

### 16 fragment 生命周期及管理

### 17 multidex 的实现原理及优化

### 18 app 编译及打包流程

### 18 android 源码中的设计模式
