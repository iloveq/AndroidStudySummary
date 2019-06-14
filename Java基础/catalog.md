### 1：jvm 内存模型

![内存模型](https://github.com/woaigmz/java-study/blob/master/jvm%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F.png)

### 2：java 垃圾回收机制/几种垃圾回收算法


### 3：类加载机制
```
①加载 - ②验证 - ③准备 - ④解析 - ⑤初始化
loading | verify prepare resolve | init unload
加载：通过类完整路径找到class二进制文件，再将二进制静态存储结构转化为方法区运行时数据结构，
并创建一个Class对象用于方法区数据结构引用入口

class 文件来源 jar包，网络，文件生成
类加载器 Bootstrap Extension Application Custom / ClassLoader
双亲委派原则

验证：class 文件安全性和正确性 文件格式/元数据/字节码

准备：为 *类变量* 内存分配和初始化默认值

解析：虚拟机将常量池里的符号引用（类和接口的全限定名以及字段和方法的名称与描述符）转为直接引用的过程

初始化：执行 java 代码进行相关初始化
```
### 4：volatile 关键字
```
C/C++ 中的 volatile 关键字和 const 对应，用来修饰变量，通常用于建立语言级别的 memory barrier。
eg：
volatile int i = 5;
int a = i;
... 一堆代码
int b = i;
编译器会对代码执行进行优化，当执行到 b 的赋值 发现没有对 i 进行修改操作就会把上次读取的 i 的值,
此时已经在从寄存器里的值赋值给 b ，而 volatile 会让编译器重新从它所在的内存读取数据
尤其在多线程访问下，一个线程修改了值，另一个线程会直接拿到修改后的内存，
也就是说让改变后的值对其它线程 visible ，此时就防止了一个线程从内存里读取，一个线程从寄存器种读取

```
### 5：线程和线程池
```
线程是操作系统可执行最小单元，位于进程内部，每个线程都要各自调用栈、寄存器、本地存储。java main()函数默认在主线程，创建新的线程，就产生一个新的调用栈。![线程有几种运行状态]()


```
### 6：几种 java 引用(强  软  弱  虚)
```
强引用（StrongReference）：是使用最普遍的引用。如果一个对象具有强引用,那垃圾回收器绝不会回收它, 当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题,强引用在方法体里，方法弹栈对象回收，当引用为全局变量要尽量手动置空 eg：
private transient Object[] elementData;
public void clear() {
        modCount++;
        // Let gc do its work
        for (int i = 0; i < size; i++)
            elementData[i] = null;
        size = 0;
}

软引用（SoftReference）：如果内存空间足够，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。
If(JVM.内存不足()) {
   str = null;  // 转换为软引用
   System.gc(); // 垃圾回收器进行回收
}

弱引用（WeakReference）：在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。

虚引用（PhantomReference）:如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。虚引用主要用来跟踪对象被垃圾回收器回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。

```


### 7：synchronized 的理解和使用


### 8：HashMap 数据结构,及实现
