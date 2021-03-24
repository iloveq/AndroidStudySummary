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
7 flutter 类上框架，好处
8 flutter 具体优化工作
9 flutter 日志上报
10 flutter webview 的坑
11 安卓中的 GcRoot

### 2021.3.21



