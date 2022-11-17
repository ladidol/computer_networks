## 前言：



- 首先是前菜 「 IP 基本认识 」
- 其次是主菜 「IP 地址的基础知识」
- 最后是点心 「IP 协议相关技术」



![IP 基础知识全家桶](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/1.jpg)



## 前菜——IP基础认识

IP 在 TCP/IP 参考模型中处于第三层，也就是**网络层**。

网络层的主要作用是：**实现主机与主机之间的通信，也叫点对点（end to end）通信。**

![IP 的作用](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/2.jpg)



### 网络层和数据链路层有什么关系呢？

MAC 的作用则是实现「直连」的两个设备之间通信，而 IP 则负责在「没有直连」的两个网络之间进行通信传输。

图示：（MAC地址和IP地址可以类比飞机票or地铁票和行程表的关系）

![IP 的作用与 MAC 的作用](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/3.jpg)

## 主菜——IP地址的基础知识

IP 地址（IPv4 地址）由 `32` 位正整数来表示，IP 地址在计算机是以二进制的方式处理的。

而人类为了方便记忆采用了**点分十进制**的标记方式



![每块网卡可以分配一个以上的IP地址](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/6.jpg)



### 1）IP地址的分类



最开始的分类地址：

 ![IP 地址分类](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/7.jpg)

其中对于 A、B、C 类主要分为两个部分，分别是**网络号和主机号**

![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/8.jpg)

- 主机号全为 1 指定某个网络下的所有主机，用于广播
- 主机号全为 0 指定某个网络

路由器默认进行**本地广播**，默认不开启**直接广播**



> 多播地址用于什么？

多播用于**将包发送给特定组内的所有主机。**



![单播、广播、多播通信](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/13.jpg)

#### IP分类的优点：

路由器和主机解析ip地址的时候更容易判断是那一类地址，从而更容易更快拿到网络地址和主机地址，**简单明了、选路（基于网络地址）简单**。

![IP 分类判断](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/14.jpg)

#### IP分类的缺点：

缺点一：

**同一网络下没有地址层次**，比如一个公司里用了 B 类地址，但是可能需要根据生产环境、测试环境、开发环境来划分地址层次，而这种 IP 分类是没有地址层次划分的功能，所以这就**缺少地址的灵活性**。



缺点二：

A、B、C类有个尴尬处境，就是**不能很好的与现实网络匹配**。

- C 类地址能包含的最大主机数量实在太少了，只有 254 个，估计一个网吧都不够用。
- 而 B 类地址能包含的最大主机数量又太多了，6 万多台机器放在一个网络下面，一般的企业基本达不到这个规模，闲着的地址就是浪费。

这两个缺点，都可以在 `CIDR` 无分类地址解决。



### 2）无分类地址 CIDR

这种方式不再有分类地址的概念，32 比特的 IP 地址被划分为两部分，前面是**网络号**，后面是**主机号**。

> 怎么划分网络号和主机号的呢？

表示形式 `a.b.c.d/x`，其中 `/x` 表示前 x 位属于**网络号**， x 的范围是 `0 ~ 32`，这就使得 IP 地址更加具有灵活性。

![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/15.jpg)

还有另一种划分网络号与主机号形式，那就是**子网掩码**，掩码的意思就是掩盖掉主机号，剩余的就是网络号。

**将子网掩码和 IP 地址按位与运算，就可得到网络号。**

![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/16.jpg)

> 为什么要分离网络号和主机号？

因为两台计算机要通讯，首先要判断是否处于同一个广播域内，即网络地址是否相同。如果网络地址相同，表明接受方在本网络上，那么可以把数据包直接发送到目标主机。

路由器寻址工作中，也就是通过这样的方式来找到对应的网络号的，进而把数据包转发给对应的网络内。

![IP地址的网络号](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/17.jpg)





> 怎么进行子网划分？

![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/18.jpg)



![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/19.jpg)

![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/21.jpg)

### 3）公有IP地址与私有IP地址

在 A、B、C 分类地址，实际上有分公有 IP 地址和私有 IP 地址。

![img](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/22.jpg)

你学校的某个私有 IP 地址和我学校的可以是一样的。

公有 IP 地址是有个组织统一分配的，假设你要开一个博客网站，那么你就需要去申请购买一个公有 IP，这样全世界的人才能访问。并且公有 IP 地址基本上要在整个互联网范围内保持唯一。

![公有 IP 地址与私有 IP 地址](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/23.jpg)



### 4）IP地址与路由控制

一个路由器一般存在于两个网络中，所以拥有两个ip地址，路由器是实现数据包跨网络传递的端口。

交换机可以在同一网络中将数据包转发到目的主机。



![IP 地址与路由控制](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/25.jpg)



> 环回地址是不会流向网络的

环回地址是在同一台计算机上的程序之间进行网络通信时所使用的一个默认地址。

计算机使用一个特殊的 IP 地址 **127.0.0.1 作为环回地址**。与该地址具有相同意义的是一个叫做 `localhost` 的主机名。使用这个 IP 或主机名时，数据包不会流向网络。

### 5）IP分片与重组

因为数据链路的最大传输单元MTU（以太网默认1500字节）的限制，IP数据包很大的话就会被分片。

值得注意的是数据包重组只能由目标主机进行，路由器是不会进行重组的。

![分片与重组](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/26.jpg)



在分片传输中，一旦某个分片丢失，则会造成整个 IP 数据报作废，所以 TCP 引入了 `MSS` 也就是在 TCP 层进行分片不由 IP 层分片，那么对于 UDP 我们尽量不要发送一个大于 `MTU` 的数据报文。



### 6）IPv6的基本认识

IPv4 大约可以提供 42 亿个地址，IPv6 可以保证地球上的每粒沙子都能被分配到一个 IP 地址。

但 IPv6 除了有更多的地址之外，还有更好的安全性和扩展性，说简单点就是 IPv6 相比于 IPv4 能带来更好的网络体验。

> IPv6地址的表示方法

IPv6 地址长度是 128 位，是以每 16 位作为一组，每组用冒号 「:」 隔开。

![IPv6 地址表示方法](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/27.jpg)



同时还可以省略链式出现的0

![Pv6 地址缺省表示方](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/28.jpg)

> IPv6地址的结构

![IPv6地址结构](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/29.jpg)

> IPv6单播地址类型

对于一对一通信的 IPv6 地址，主要划分了三类单播地址，每类地址的有效范围都不同。

- 在同一链路单播通信，不经过路由器，可以使用**链路本地单播地址**，IPv4 没有此类型
- 在内网里单播通信，可以使用**唯一本地地址**，相当于 IPv4 的私有 IP
- 在互联网通信，可以使用**全局单播地址**，相当于 IPv4 的公有 IP

![ IPv6 中的单播通信](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/30.jpg)



### 7）IPv4 首部与 IPv6 首部

![IPv4 首部与 IPv6 首部的差异](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/31.jpg)

##  点心 —— IP 协议相关技术

- DNS 域名解析
- ARP 与 RARP 协议
- DHCP 动态获取 IP 地址
- NAT 网络地址转换
- ICMP 互联网控制报文协议
- IGMP 因特网组管理协

### DNS

**DNS域名解析**，DNS 可以将域名网址自动转换为具体的 IP 地址。

> 域名的层级关系

- 根 DNS 服务器
- 顶级域 DNS 服务器（com）
- 权威 DNS 服务器（server.com）

![DNS 树状结构](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/32.jpg)



根域的 DNS 服务器信息保存在互联网中所有的 DNS 服务器中。这样一来，任何 DNS 服务器就都可以找到并访问根域 DNS 服务器了。

> 域名解析的工作流程



浏览器首先看一下自己的缓存里有没有，如果没有就向操作系统的缓存要，还没有就检查本机域名解析文件 `hosts`，如果还是没有，就会 DNS 服务器进行查询，

![域名解析的工作流程](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/33.jpg)



DNS 域名解析的过程蛮有意思的，整个过程就和我们日常生活中找人问路的过程类似，**只指路不带路**。

### ARP

由于主机的路由表中可以找到下一跳的 IP 地址，所以可以通过 **ARP 协议**，求得下一跳的 MAC 地址。

> 那么 ARP 又是如何知道对方 MAC 地址的呢？

简单地说，ARP 是借助 **ARP 请求与 ARP 响应**两种类型的包确定 MAC 地址的。

![ARP 广播](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/34.jpg)



一般操作系统会将mac地址有期限的缓存起来的。

> RARP 协议你知道是什么吗？

这个一般是打印机服务器等小型嵌入式设备接入到网络是就经常会用到。

![RARP](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/35.jpg)



### DHCP

我们的电脑通过DHCP动态获取IP地址，大大省去了配IP信息的繁琐过程。

![DHCP 工作流程](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/36.jpg)



可以发现，DHCP 交互中，**全程都是使用 UDP 广播通信**。

> 咦，用的是广播，那如果 DHCP 服务器和客户端不是在同一个局域网内，路由器又不会转发广播包，那不是每个网络都要配一个 DHCP 服务器？

![ DHCP 中继代理](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/37.jpg)



因此，DHCP 服务器即使不在同一个链路上也可以实现统一分配和管理IP地址。

### NAT

提出了一种**网络地址转换 NAT** 的方法，再次缓解了 IPv4 地址耗尽的问题。

简单的来说 NAT 就是同个公司、家庭、教室内的主机对外部通信时，把私有 IP 地址转换成公有 IP 地址

![NAT](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/38.jpg)



> 那不是 N 个私有 IP 地址，你就要 N 个公有 IP 地址？这怎么就缓解了 IPv4 地址耗尽的问题？这不瞎扯吗？

确实是，普通的 NAT 转换没什么意义。

由于绝大多数的网络应用都是使用传输层协议 TCP 或 UDP 来传输数据的。

因此，可以把 IP 地址 + 端口号一起进行转换。

这样，就用一个全球 IP 地址就可以了，这种转换技术就叫**网络地址与端口转换 NAPT。**

很抽象？来，看下面的图解就能瞬间明白了。

![NAPT](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/39.jpg)

**两个私有 IP 地址都转换 IP 地址为公有地址 120.229.175.121，但是以不同的端口号作为区分。**

> NAT 那么牛逼，难道就没缺点了吗？

由于 NAT/NAPT 都依赖于自己的转换表，因此会有以下的问题：

- 外部无法主动与 NAT 内部服务器建立连接，因为 NAPT 转换表没有转换记录。
- 转换表的生成与转换操作都会产生性能开销。
- 通信过程中，如果 NAT 路由器重启了，所有的 TCP 连接都将被重置。

> 如何解决 NAT 潜在的问题呢？

*第一种就是改用 IPv6*

*第二种 NAT 穿透技术*

### ICMP

ICMP 全称是 **Internet Control Message Protocol**，也就是**互联网控制报文协议**。

主要是在网络包出问题进行报错操作。

> ICMP 功能都有啥？

`ICMP` 主要的功能包括：**确认 IP 包是否成功送达目标地址、报告发送过程中 IP 包被废弃的原因和改善网络设置等。**

![ICMP 目标不可达消息](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/40.jpg)



> ICMP类型



ICMP 大致可以分为两大类：

- 一类是用于诊断的查询消息，也就是「**查询报文类型**」
- 另一类是通知出错原因的错误消息，也就是「**差错报文类型**」

![常见的 ICMP 类型](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/41.jpg)

### IGMP

![组播模型](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/42.jpg)

**IGMP 是因特网组管理协议，工作在主机（组播成员）和最后一跳路由之间**，如上图中的蓝色部分。

> IGMP工作机制

IGMP 分为了三个版本分别是，IGMPv1、IGMPv2、IGMPv3。

接下来，以 `IGMPv2` 作为例子，说说**常规查询与响应和离开组播组**这两个工作机制。

***常规查询与响应工作机制***

![ IGMP 常规查询与响应工作机制](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/43.jpg)







***离开组播组工作机制***

情况一：网段中仍有该组播组（就是组是否为空

![ IGMPv2 离开组播组工作机制 情况1](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/44.jpg)



情况二：网段中没有该组播组：（组是否为空

![ IGMPv2 离开组播组工作机制 情况2](https://figurebed-ladidol.oss-cn-chengdu.aliyuncs.com/img/45.jpg)
