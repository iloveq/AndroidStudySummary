### 1：jvm 内存模型

![内存模型](https://github.com/woaigmz/java-study/blob/master/jvm%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F.png)

### 2：java 垃圾回收机制/几种垃圾回收算法


### 3：类加载机制

①加载 - ②验证 - ③准备 - ④解析 - ⑤初始化

加载：通过类完整路径找到class二进制文件，再将二进制静态存储结构转化为方法区运行时数据结构，
并创建一个Class对象用于方法区数据结构引用入口

class 文件来源 jar包，网络，文件生成
类加载器 Bootstrap Extension System Custom / ClassLoader
双亲委派原则

验证：class 文件安全性和正确性 文件格式/元数据/字节码

准备：为 *类变量* 内存分配和初始化默认值

解析：虚拟机将常量池里的符号引用（类和接口的全限定名以及字段和方法的名称与描述符）转为直接引用的过程

初始化：执行 java 代码进行相关初始化

### 4：volatile 关键字


### 5：线程和线程池


### 6：几种 java 引用(强 弱 软 虚)


### 7：synchronized 的理解和使用


### 8：HashMap 数据结构,及实现
