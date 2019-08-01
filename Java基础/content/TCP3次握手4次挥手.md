### TCP报文格式图：
![TCP报文格式图：](https://github.com/woaigmz/java-study/blob/master/TCP%E6%8A%A5%E6%96%87.png)
### 字段介绍：
##### （1）Seq序号：32位，用来标识从TCP源端向目的端发送的字节流，发起方发送数据时对此进行标记
##### （2）确认序号：Ack序号，占32位，只有ACK标志位为1时，确认序号字段才有效，Ack=Seq+1
##### （3）标志位：共6个，即URG、ACK、PSH、RST、SYN、FIN等，具体含义如下：

```
       （A）URG：紧急指针（urgent pointer）有效。
       （B）ACK：确认序号有效。
       （C）PSH：接收方应该尽快将这个报文交给应用层。
       （D）RST：重置连接。
       （E）SYN：发起一个新连接。
       （F）FIN：释放一个连接。
```

#####  三次握手： 老外问路

  foreign：are you speak english?
  
  you：yes.
  
  foreign：where the ...
  
#####  三次握手流程：
![三次握手流程图：](https://github.com/woaigmz/java-study/blob/master/3%E6%AC%A1%E6%8F%A1%E6%89%8B.png)

```
（1）第一次握手：
    Client将标志位SYN置为1，随机产生一个值seq=J，并将该数据包发送给Server，Client进入SYN_SENT状态，等待Server确认。
（2）第二次握手：
    Server收到数据包后由标志位SYN=1知道Client请求建立连接，Server将标志位SYN和ACK都置为1，ack=J+1，随机产生一个值seq=K，并将该数据包发送给Client以确认连接请求，Server进入SYN_RCVD状态。
（3）第三次握手：
    Client收到确认后，检查ack是否为J+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=K+1，并将该数据包发送给Server，Server检查ack是否为K+1，ACK是否为1，如果正确则连接建立成功，Client和Server进入ESTABLISHED状态，完成三次握手，随后Client与Server之间可以开始传输数据了。
```

##### SYN攻击：

```
在三次握手过程中，Server发送SYN-ACK之后，收到Client的ACK之前的TCP连接称为半连接（half-open connect），
此时Server处于SYN_RCVD状态，当收到ACK后，Server转入ESTABLISHED状态。
SYN攻击就是Client在短时间内伪造大量不存在的IP地址，并向Server不断地发送SYN包，Server回复确认包，
并等待Client的确认，由于源地址是不存在的，因此，Server需要不断重发直至超时，这些伪造的SYN包将产时间占用未连接队列，
导致正常的SYN请求因为队列满而被丢弃，从而引起网络堵塞甚至系统瘫痪。SYN攻击时一种典型的DDOS攻击，检测SYN攻击的方式非常简单，
即当Server上有大量半连接状态且源IP地址是随机的，则可以断定遭到SYN攻击了，使用如下命令可以让之现行：
# netstat -nap | grep SYN_RECV
```

#####  四次挥手流程：
所谓四次挥手（Four-Way Wavehand）即终止TCP连接，就是指断开一个TCP连接时，
由于TCP连接时全双工的，因此，每个方向都必须要单独进行关闭需要客户端和服务端总共发送4个包以确认连接的断开

![四次挥手流程图：](https://github.com/woaigmz/java-study/blob/master/4%E6%AC%A1%E6%8C%A5%E6%89%8B.png)

```
（1）第一次挥手：
    Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。
（2）第二次挥手：
    Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态。
（3）第三次挥手：
    Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。
（4）第四次挥手：
    Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手
```


