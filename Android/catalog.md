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
- js原生交互,调原生的方法是在子线程中执行

### 5 探讨 android 进程，进程间通信方式，多进程的应用？
进程跟线程的区别，进程是一般指的是一个执行单元,线程是cpu调度的最小单元,一个进程可以包括多个线程。
进程间通信就是IPC。IPC不是android特有的。任何一个操作系统都有响应的IPC。

 Windows上可以通过剪切板、管道和邮槽等进程间通信,Linux可以通过命名管道、共享内容、信号量等通信。虽然android基于linux内核的移动操作系统,他的通信方式并不能完全继承Linux,他有自己的进程通信方式，android 多进程设置android:process。

android 进程间通信方式
- Bundle
- 文件共享
- Messenger
- AIDL
- ContentProvider
- Socket

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

### 19 android 源码中的设计模式

### 20 性能优化的几种角度(UI 内存 网络 数据 耗电) 优化工具及方法

### 21 Webview 初始化源码分析 

### 22 SharePreference 源码分析

### 23 Activity 启动模式
四种常见启动模式 singleTask singleTop singleInstance standard
几种activity属性
- 1.android:allowTaskReparenting 这个属性用来标记一个Activity实例在当前应用退居后台后，是否能从启动它的那个task移动到有共同affinity的task，“true”表示可以移动，“false”表示它必须呆在当前应用的task中，默认值为false。如果一个这个Activity的元素没有设定此属性，设定在上的此属性会对此Activity起作用。例如在一个应用中要查看一个web页面，在启动系统浏览器Activity后，这个Activity实例和当前应用处于同一个task，当我们的应用退居后台之后用户再次从主选单中启动应用，此时这个Activity实例将会重新宿主到Browser应用的task内，在我们的应用中将不会再看到这个Activity实例，而如果此时启动Browser应用，就会发现，第一个界面就是我们刚才打开的web页面，证明了这个Activity实例确实是宿主到了Browser应用的task内。 

- 2.android:alwaysRetainTaskState 这个属性用来标记应用的task是否保持原来的状态，“true”表示总是保持，“false”表示不能够保证，默认为“false”。此属性只对task的根Activity起作用，其他的Activity都会被忽略。 默认情况下，如果一个应用在后台呆的太久例如30分钟，用户从主选单再次选择该应用时，系统就会对该应用的task进行清理，除了根Activity，其他Activity都会被清除出栈，但是如果在根Activity中设置了此属性之后，用户再次启动应用时，仍然可以看到上一次操作的界面。 这个属性对于一些应用非常有用，例如Browser应用程序，有很多状态，比如打开很多的tab，用户不想丢失这些状态，使用这个属性就极为恰当。 

- 3.android:clearTaskOnLaunch 这个属性用来标记是否从task清除除根Activity之外的所有的Activity，“true”表示清除，“false”表示不清除，默认为“false”。同样，这个属性也只对根Activity起作用，其他的Activity都会被忽略。 如果设置了这个属性为“true”，每次用户重新启动这个应用时，都只会看到根Activity，task中的其他Activity都会被清除出栈。如果我们的应用中引用到了其他应用的Activity，这些Activity设置了allowTaskReparenting属性为“true”，则它们会被重新宿主到有共同affinity的task中。 

- 4.android:finishOnTaskLaunch 这个属性和android:allowReparenting属性相似，不同之处在于allowReparenting属性是重新宿主到有共同affinity的task中，而finishOnTaskLaunch属性是销毁实例。如果这个属性和android:allowReparenting都设定为“true”，则这个属性好些。

- 5.android:taskAffinity 在Android系统中，一个application的所有Activity默认有一个相同的affinity（亲密关系,相似之处）。也就是说同一个应用程序的的所有Activity倾向于属于同一个task。但是我们并不能说Android里一个应用程序只有一个任务栈。taskAffinity 可以指定 任务栈,但是通过 NEWTASK启动 activity 会把所有系统不同app进程相同taskAffinity的吸附到一个task里
