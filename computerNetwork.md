# 计算机网络知识总结（未完待续...)

<ul>
<li><a href="#%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E7%9F%A5%E8%AF%86%E6%80%BB%E7%BB%93%E6%9C%AA%E5%AE%8C%E5%BE%85%E7%BB%AD">计算机网络知识总结（未完待续...)</a>
<ul>
<li><a href="#1-%E6%A6%82%E8%BF%B0">1. 概述</a></li>
<li><a href="#2-%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82">2. 数据链路层</a>
<ul>
<li><a href="#21-ppp%E5%8D%8F%E8%AE%AE">2.1 PPP协议</a></li>
<li><a href="#22-csmacd%E5%8D%8F%E8%AE%AE%E4%BB%A5%E5%A4%AA%E7%BD%91">2.2 CSMA/CD协议（以太网）</a></li>
</ul>
</li>
<li><a href="#3-%E7%BD%91%E7%BB%9C%E5%B1%82">3. 网络层</a>
<ul>
<li><a href="#31-%E7%BD%91%E9%99%85%E5%8D%8F%E8%AE%AE-ip">3.1 网际协议 IP</a>
<ul>
<li><a href="#311-%E6%9C%80%E5%9F%BA%E6%9C%AC%E7%9A%84%E5%88%86%E7%B1%BB%E7%9A%84ip%E5%9C%B0%E5%9D%80">3.1.1 最基本的分类的IP地址</a></li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>

## 1. 概述

* 三大类网络：电信网络、有线电视网络、计算机网络
<br>

* 互联网的两个重要基本特点：
  - **连通性**：就是互联网使上网用户之间，不管相距多远，都可以非常便捷、非常经济的交换各种信息，好像这些用户终端都彼此直接连通一样。
  - **共享**：共享就是资源共享。可以是信息共享、软件共享，也可以是应将共享。
<br>

* 计算机网络（简称网络）由若干 **结点(node)** 和连接这些节点的 **链路(link)** 组成。
  - 按照**网络的作用范围**进行分类：广域网 WAN (Wide    Area NetWork)、城域网 MAN (Metropolitan Area     Network)、局域网 LAN (Local Area Network)、个人   局域网 PAN (Personal Area Network)。
  - 按照**网络的使用者**进行分类：公用网 (public nerwork)、专用网 (private network)。
<br>

* 计算机网络的性能
  - 计算机网络的性能指标：速率、带宽、吞吐量、时延、时延带宽积、往返时间RTT、利用率；
  - 计算机网络的非性能特征：费用、质量、标准化、可靠性、可扩展性和可升级性、易于管理和维护。
<br>

* **internet（互连网）** 是一个通用名词，它泛指有多个计算机网络互连而成的计算机网络；
  **Internet（互联网，或因特网）** 则是一个专用名词，它指当前全球最大的、开放的、有众多互联网络相互连接而成的特定互连网，它采用 TCP/IP 协议族作为通信的规则，且其前身是美国的ARPANET。
<br>

* 从工作方式上看，互联网可以划分为 **边缘部分** （由所有介在互联网上的主机组成）和 **核心部分** （由大量网络和连接这些网络的路由器组成）。
<br>

* 互联网的核心部分：
  在网络核心部分起特殊作用的是 **路由器(router)** ，它是一种专用计算机（但不叫做主机）。路由器是实现**分组交换(packet switiching)** 的关键构件，其任务是**转发收到的分组**，这是网络核心部分最重要的内容。
<br>

* 分组交换采用 **存储转发** 技术。
  优点：
<table>
<thead>
<tr>
<th>优点</th>
<th style="text-align:center">所采用的手段</th>
</tr>
</thead>
<tbody>
<tr>
<td>高效</td>
<td style="text-align:center">在分组传输的过程中动态分配传输带宽，对通信链路是逐段占用</td>
</tr>
<tr>
<td>灵活</td>
<td style="text-align:center">为每一个分组独立的选择最合适的转发路由</td>
</tr>
<tr>
<td>迅速</td>
<td style="text-align:center">以分组作为传送单位，可以不先建立连接就能向其他主机发送分组</td>
</tr>
<tr>
<td>可靠</td>
<td style="text-align:center">保证可靠性的网络协议：分布式多路由的分组交换网，使网络有很好的生存性</td>
</tr>
</tbody>
</table>
  
  缺点：
  **时延**：分组在各路由器存储转发时需要排队，造成一定时延；
  **开销**：各分组必须携带的控制信息造成了一定的开销。
<br>

* 计算机网络体系结构
<table>
<thead>
<tr>
<th style="text-align:center">OSI的体系结构</th>
<th style="text-align:center">TCP/TP的体系结构</th>
<th style="text-align:center">五层协议的体系结构</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">应用层</td>
<td style="text-align:center">^</td>
<td style="text-align:center">^</td>
</tr>
<tr>
<td style="text-align:center">表示层</td>
<td style="text-align:center">^</td>
<td style="text-align:center">^</td>
</tr>
<tr>
<td style="text-align:center">会话层</td>
<td style="text-align:center">应用层(各种应用层协议如TELNET,FTP,SMTP等)</td>
<td style="text-align:center">应用层</td>
</tr>
<tr>
<td style="text-align:center">运输层</td>
<td style="text-align:center">运输层（TCP或UDP）</td>
<td style="text-align:center">运输层</td>
</tr>
<tr>
<td style="text-align:center">网络层</td>
<td style="text-align:center">网际层IP</td>
<td style="text-align:center">网络层</td>
</tr>
<tr>
<td style="text-align:center">数据链路层</td>
<td style="text-align:center">^</td>
<td style="text-align:center">数据链路层</td>
</tr>
<tr>
<td style="text-align:center">物理层</td>
<td style="text-align:center">网络接口层</td>
<td style="text-align:center">物理层</td>
</tr>
</tbody>
</table>
<br>

* 协议主要有以下三个要素组成：
  1. **语法**：即数据与控制信息的结构或格式；
  2. **语义**：即需要发出何种控制信息，完成何种动作以及做出何种响应；
  3. **同步**：即事件实现顺序的详细说明。

## 2. 数据链路层

* 数据链路层主要使用的两种信道类型：**点对点信道（PPP协议）** 和 **广播信道（CSMA/CD协议）**。
<br>

* 数据链路层的三个基本问题：
  1. 封装成帧：在一段数据的前后分别添加首部和尾部，这样就构成了一个帧；
  2. 透明传输：无论什么样的比特组合的数据，都能够按照原样没有差错地通过这个数据链路层；
  3. 差错检测：实际的通信链路并非是理想的，为了保证数据传输的可靠性，在计算机网络传输数据时，必须采用各种差错检测措施（循环冗余检验CRC）。

### 2.1 PPP协议

* 三个组成部分：
  1. 一个将IP数据报封装到串行链路的方法；
  2. 一个用来建立、配置和测试数据链路连接的 链路控制协议 LCP (Link Control Protocol)；
  3. 一套 网络控制协议 NCP (Network Control Protocol)，其中的每一个协议支持不同的网络层协议。
<br>

* PPP帧格式：
<table>
<thead>
<tr>
<th style="text-align:center"></th>
<th style="text-align:center">F</th>
<th style="text-align:center">A</th>
<th style="text-align:center">C</th>
<th style="text-align:center">协议</th>
<th style="text-align:center">信息部分</th>
<th style="text-align:center">FCS</th>
<th style="text-align:center">F</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">字节</td>
<td style="text-align:center">1</td>
<td style="text-align:center">1</td>
<td style="text-align:center">1</td>
<td style="text-align:center">2</td>
<td style="text-align:center">不超过1500字节</td>
<td style="text-align:center">2</td>
<td style="text-align:center">1</td>
</tr>
</tbody>
</table>

  * 标志字段 F 规定为 0x7E
  * 地址字段 A 规定为 0xFF
  * 控制字段 C 规定为 0x03
<br>

* 特点：
  1. 简单；
  2. 只检测差错，而不是纠正差错；
  3. 不使用序号，也不进行流量控制；
  4. 可同时支持多种网络层协议。
<br>

### 2.2 CSMA/CD协议（以太网）

* 计算机与外界局域网的通信要通过通信适配器（或网络适配器），它又称为网络接口卡或网卡。计算机的硬件地址就在适配器的ROM中。**MAC地址48位**。以太网的适配器有过滤功能，它只接收单波帧、广播帧和多播帧。
<br>

* 以太网采用无连接的工作方式，对发送的数据帧不进行编号，也不要求对方发回确认。目的站收到有差错帧就把它丢弃，其他什么也不做。
<br>

* 以太网所采用的协议是具有冲突检测的载波监听多点接入 CSMA/CD。协议的要点是：发送前先监听，边发送边监听，一旦发现总线上出现了碰撞，就立即停止发送。然后按照退避算法等待一段随机时间后再次发送。（“多点接入”，“载波监听”，“碰撞检测”）使用CSMA/CS协议不可能同时进行发送和接收，只能进行 **双向交替通信（半双工通信）**。

## 3. 网络层

* **网络层向上只提供简单灵活的、无连接的、尽最大努力交付的数据报服务**。**网络层不提供服务质量的承诺**，也就是说所传送的分组可能出错、丢失、重复和失序，当然也不能保证分组交付的时间。
<br>

* 将网络互相连接起来的一些中间设备：
  1. 物理层使用的中间设备叫做 **转发器(repeater)**;
  2. 数据链路层使用的中间设备叫做 **网桥** 或 **桥接器(bridge)**;
  3. 网络层使用的中间设备叫做 **路由器(router)**;
  4. 在网络层以上使用的中间设备叫做 **网关(gateway)**。用网关连接两个不兼容的系统需要在高层进行协议转换的时候。
  
  当中间设备是转发器或网桥时，这仅仅是把一个网络扩大了，而从网络层的角度看，这仍然是一个网络，一般并不称之为网络互连。

### 3.1 网际协议 IP

* 与IP协议配套使用的三个协议：
  * 地址解析协议 ARP (Address Resolution Protocol)
  * 网际控制报文协议 ICMP (Internet Control Message Protocol)
  * 网际组管理协议 IGMP (Internet Group Management Protocol)

* IP地址的编址方法共经历了三个历史阶段：**分类的IP地址**，**子网的划分**，**构成超网**。

#### 3.1.1 最基本的分类的IP地址

* A类、B类和C类地址的网络号字段分别为1个、2个和3个字节长，而在网络号字段的最前面有1~3位的类别位，其数值分别规定为0，10和110。**单播地址**（一对一通信）。
<br>

* A类、B类和C类地址的主机号字段分别位3个、2个和1个字节长。
<br>

* D类地址（前4位是 1110）用于 **多播**（一对多通信）。
<br>

* E类地址（前4位是 1111）保留为以后用。
<br>

* 一般不使用的特殊IP地址：
<table>
<thead>
<tr>
<th style="text-align:center">网络号</th>
<th style="text-align:center">主机号</th>
<th style="text-align:center">源地址使用</th>
<th style="text-align:center">目的地址使用</th>
<th style="text-align:center">代表的意思</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">0</td>
<td style="text-align:center">0</td>
<td style="text-align:center">可以</td>
<td style="text-align:center">不可</td>
<td style="text-align:center">在本网络上的本主机</td>
</tr>
<tr>
<td style="text-align:center">0</td>
<td style="text-align:center">host-id</td>
<td style="text-align:center">可以</td>
<td style="text-align:center">不可</td>
<td style="text-align:center">在本网络上的某台主机 host-id</td>
</tr>
<tr>
<td style="text-align:center">全1</td>
<td style="text-align:center">全1</td>
<td style="text-align:center">不可</td>
<td style="text-align:center">可以</td>
<td style="text-align:center">只在本网络上进行广播（各路由器均不转发）</td>
</tr>
<tr>
<td style="text-align:center">net-id</td>
<td style="text-align:center">全1</td>
<td style="text-align:center">不可</td>
<td style="text-align:center">可以</td>
<td style="text-align:center">对net-id上的所有主机进行广播</td>
</tr>
<tr>
<td style="text-align:center">127</td>
<td style="text-align:center">非全0或全1的任何数</td>
<td style="text-align:center">可以</td>
<td style="text-align:center">可以</td>
<td style="text-align:center">用于本地软件环回测试</td>
</tr>
</tbody>
</table>

  