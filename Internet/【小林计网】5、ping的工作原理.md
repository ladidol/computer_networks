## ping的工作原理

判断与对方网络是否通畅，最常用的就是`ping`命令



## IP协议的助手——ICMP协议

在IP基础知识全家桶中的点心中有谈到过ICMP协议，这里就不详细说了。。

`ICMP` 主要的功能包括：**确认 IP 包是否成功送达目标地址、报告发送过程中 IP 包被废弃的原因和改善网络设置等。**

ICMP 报文是封装在 IP 包里面，它工作在网络层，是 IP 协议的助手。



![ICMP 回送请求和回送应答报文](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/202210272211657.png)











### 查询报文类型

> 回送消息 —— 类型 `0` 和 `8`



![ICMP 回送消息](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/202210272211823.png)



### 差错报文类型

- 目标不可达消息 —— 类型 为 `3`
- 原点抑制消息 —— 类型 `4`
- 重定向消息 —— 类型 `5`
- 超时消息 —— 类型 `11`



> 目标不可达消息（Destination Unreachable Message） —— 类型为 `3`

![目标不可达类型的常见代码号](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/9.jpg)



> 原点抑制消息（ICMP Source Quench Message） —— 类型 `4`

当路由器向低速线路发送数据时，其发送队列的缓存变为零而无法发送出去时，可以向 IP 包的源地址发送一个 ICMP **原点抑制消息**。

> 重定向消息（ICMP Redirect Message） —— 类型 `5`

如果路由器发现发送端主机使用了「不是最优」的路径发送数据，那么它会返回一个 ICMP **重定向消息**给这个主机。

> 超时消息（ICMP Time Exceeded Message） —— 类型 `11`

IP 包中有一个字段叫做 `TTL` （`Time To Live`，生存周期），它的**值随着每经过一次路由器就会减 1，直到减到 0 时该 IP 包会被丢弃。**

![image-20221025192534891](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/image-20221025192534891.png)

设置 IP 包生存周期的主要目的，是为了在路由控制遇到问题发生循环状况时，避免 IP 包无休止地在网络上被转发。

## ping —— 查询报文类型的使用

接下来，我们重点来看 `ping` 的**发送和接收过程**。

同个子网下的主机 A 和 主机 B，主机 A 执行`ping` 主机 B 后，我们来看看其间发送了什么？

![主机 A ping 主机 B](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/12.jpg)

**可以图解**

![主机 A ping 主机 B 期间发送的事情](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/17.png)



当然这只是最简单的，同一个局域网里面的情况。如果跨网段的话，还会涉及网关的转发、路由器的转发等等。

但是对于 ICMP 的头来讲，是没什么影响的。会影响的是根据目标 IP 地址，选择路由的下一跳，还有每经过一个路由器到达一个新的局域网，需要换 MAC 头里面的 MAC 地址。

说了这么多，可以看出 ping 这个程序是**使用了 ICMP 里面的 ECHO REQUEST（类型为 8 ） 和 ECHO REPLY （类型为 0）**。



##  traceroute —— 差错报文类型的使用

Windows中对等的命令叫做 `tracert`



***1. traceroute 作用一***

traceroute 的第一个作用就是**故意设置特殊的 TTL，来追踪去往目的地时沿途经过的路由器。**

举个例子：`tracert www.ladidol.top`

![image-20221025194229646](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/image-20221025194229646.png)

***2. traceroute 作用二***

traceroute 还有一个作用是**故意设置不分片，从而确定路径的 MTU**。

![MTU 路径发现（UDP的情况下）](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/18.jpg)







