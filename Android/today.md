### 2021.2.20

1 .9图 As绘制，NinePatchDrawable，原理 :byte[] chunk draw （android/ios）

2 资源混淆如何影响kotlin协程，serviceloader - fastloader

3 flutter 技术背景，技术对比，核心原理，widgettree - elementtree - rendertree - layer - sence - surfaceview/textureview 混合开发

4 flutter manifest 2个 metedata [ theme，splashScreendrawable ]

### 2021.2.23

1 从 setContentView 分析 surface 在 activity 的创建过程

2 toast 源码分析 tn wms 交互，badtoken 问题原因，解决方法

3 contentprovider 源码分析 主动启动 ，被动启动（是否多进程模式） 使用query方法(cursor)数据传递过程 匿名共享内存 / 使用call方法(binder机制)

### 2021.2.25

1 从 aidl 到 binder 机制   Client - Server - ServiceManager binder 的关系

### 2021.2.26

1 Dialog 的创建 window token 是一种什么样的存在并从源码设计角度分析，SystemService -> AMS/WMS - create token/token verify，context - application/service/activity

2 从事件如何到达 activity 到事件分发机制

### 2021.3.10

1 删除字符串中所有相邻重复的字符

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
题解：
class Solution{
    public static String removeDuplicates(String S){
        StringBuffer sb = new StringBuffer();
        int top = -1;
        for(int i = 0; i < S.length(); ++i){
            Char ch = S.charAt(i);
            if(i>=0&&sb.charAt(top)==ch){
                sb.deleteCharAt(top);
                i --;
            }else{
                sb.append(ch);
                i ++;
            }
        }
        return sb.toString()
    }
}

上面问题:sb.chartAt(top)这个top没有变，一直-1，估计要有top--and top++;

可以这样吧
class Solution{
    public static String removeDuplicates(String S){
        StringBuffer sb = new StringBuffer();
        int top = 0;
        for(int i = 0; i < S.length(); i++){
            char ch = S.charAt(i);
            if(top>0&&sb.charAt(top-1)==ch){
				top--;
                sb.deleteCharAt(top);
            }else{
				top++;
                sb.append(ch);
            }
        }
        return sb.toString()
    }
}

```
    
### 2021.3.11

1 返回倒数第 k 个节点

```
实现一种算法，找出单向链表中倒数第 k 个节点。返回该节点的值。

注意：本题相对原题稍作改动

示例：

输入： 1->2->3->4->5 和 k = 2
输出： 4
说明：

给定的 k 保证是有效的。

题解：

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */

class Solution{

public int kthToLast(ListNode head, int k) {
        ListNode first = head;
        ListNode second = head;
        //第一个指针先走k步
        while (k-- > 0) {
            first = first.next;
        }
        //然后两个指针在同时前进
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        return second.val;
    }

}

```
### 2021.3.17
1 hashmap 的源码分析 hask 算法  putVal/resize 

百度阅读一面

1  activity 启动流程 ActivityThread main 方法怎么调用

2 anr 如何检测

3 okhttp 相关

4 leakcanary 原理

5 view layout 过程

6 handler 如何处理延时消息

7 threadlocal 数据结构

8 java gc 垃圾回收

9 线程池

### 2021.3.18

美团打车1面

1  private protected public

2 string s1 = new String(“a”); string s2 = “a”; s1 == s2

3 安卓内存泄露有哪些

4 单例模式

5 jvm 内存垃圾回收

6 安卓view 宽高什么时候得到

7 flutter 类似框架，好处

8 flutter 具体优化工作

9 flutter 日志上报

10 flutter webview 的坑

11 安卓中的 GcRoot

### 2021.3.21

华为荣耀一面

1 算法：

```
题目描述
给定一个数组arr，返回子数组的最大累加和
例如，arr = [1, -2, 3, 5, -2, 6, -1]，所有子数组中，[3, 5, -2, 6]可以累加出最大的和12，所以返回12.
[要求]
时间复杂度为O(n)O(n)，空间复杂度为O(1)O(1)

示例
输入
[1, -2, 3, 5, -2, 6, -1]
输出
12

Solution:
 public int maxsumofSubarray (int[] arr) {
        // write code here
        int[] arr1 = new int[arr.length+1];
        int max = Integer.MIN_VALUE;
        for(int i=0;i<arr.length;i++){
            arr1[i+1] = arr[i]+arr1[i];
            if(arr1[i+1]<0){
                arr1[i+1] = 0;
            }
            max = Math.max(arr1[i+1],max);
        }
        return max;
    }

```
2 安卓从应用开机到启动Launcher

3 从点击Launcher图标到Activity启动过程

4 说一说性能优化到几个方向

华为荣耀二面

1 binder 机制

2 系统服务有那些，怎么创建的

3 java 内存区域

4 垃圾回收算法

5 说一说 handler 消息机制

字节幸福里一面

1 java volatile 和 synchorniced 区别

2 算法 给一个数组，给一个 k 值，输出所有相加为 k 的子数组

3 java 垃圾回收算法

4 安卓gcRoot

5 安卓内存泄漏检测线上方法

6 aidl 中的 anr 遇到过吗

8 安卓侧滑返回怎么实现

9 recyclerview 缓存机制是什么样的

10 安卓4中启动模式

百度阅读二面

1 java 中 error 和 exception 有什么区别

2 算法： 如何判断链表是环，并计算出环的长度

3 安卓 图片 glide 框架怎么缓存的，lru 数据结构

4 安卓中 IntentService

5 讲下安卓 aidl

6 安卓中几种启动模式及其应用场景

7 项目中 jsbride 怎么设计的

# 2021.3.23

小米一面

1 java hashmap 如何扩容，hashmap 如何存放数据，红黑树和二叉平衡树的区别

2 java volatile 和 synchronized 区别

3 java 死锁如何造成

4 bitmap 如何计算大小 

5 算法 ：二维数组 [[1,2,3],[4,5,6],[7,8,9]] 输出 [[1,4,7],[2,5,8],[3,6,9]] 

6 handler 消息机制内存屏障

7 安卓电量优化，应用在后台耗电怎么检测

8 安卓 koltin 了解么，携程和rxjava区别

9 应用包活方法

# 2021.3.25

字节幸福里二面

1 flutter 中 网络框架，图片框架，实现原理 类比 okhttp glide，图片是否可以互相使用

2 flutter 项目中解决的最有成就感的问题

3 flutter statewidget 生命周期

3 算法 返回倒数第 k 个节点 最近这个题 hot

4 安卓 handler 消息机制

5 手写几种 java 单例模式 doublecheck，volatile synchronized 区别

6 flutter 混合开发了解过吗，聊一下

7 未来想继续 flutter 还是 android （2个平台的优缺点）

# 2021.3.26

京东云一面

1 flutter，reactnative 区别

2 mvp ， mvvm 区别

3 jitpack 有哪些

4 kotlin 扩展函数，kotlin run let 区别

5 activity 几种启动模式，及其使用场景

6 用过 vue 吗，简单介绍下

7 java 垃圾回收机制

8 性能优化做过什么

京东云二面

1 activity 以 singleTop 模式启动，如果顶层页面崩溃会发生什么，如何处理

2 activity 生命周期

3 fragment 生命周期

4 service 生命周期

5 contentprovider 和 application 的 oncreate 谁先执行

6 降下性能优化中用到的排查工具

# 2020.3.29

美团打车二面

1 integer(1) == interger(2)

2 arraylist linkedlist 区别

3 hashmap 扩容，线程同步 map 有那些

4 sparearray arraymap 区别

5 java 内存模型

6 java 内存区域划分

7 算法 ：写3种简单排序

8 举例几种内存泄漏的例子，leakcannery 原理

9 聊聊安卓启动优化

10 安卓 activity 生命周期

11 安卓 intentservice


字节幸福里3面

1 项目里写到 activity 启动传参及返回接受参数框架的设计

2 设计一个 埋点框架

3 项目里写到设计模式的使用

4 项目中遇到的坑及其优化解决方法

5 项目里性能方面的优化点

6 项目里插件化怎么做的

字节幸福里4面

1 3段工作经历里项目中担任的角色和责任

2 3段工作经历里项目每个项目介绍及自己负责部分详细介绍

3 最有成就感的工作

4 职业规划






