---
layout: default
title:  "TCP连接的建立与释放"
author: itsmikej
category: TCP/IP
---

#### TCP 状态机

![tcpstatus][1]

#### 建立和释放连接

![open&close][2]

有几点需要注意的

#### 为什么是3次握手，而不是2次，4次？

 - 理论上多少次都不够，因为最后一次不能确保收到，3次基本能保证连接可靠。
 - 两次风险又太高，因为 server 收到请求就建立了链接，如果响应的 ACK client 没收到，server 会一直等待，浪费资源。

#### 释放链接为什么是4次挥手？

TCP 是全双工协议，需要确保双方都断开链接。（解释得好牵强。。）

#### 释放时为什么会有TIME-WAIT状态？

确保最后一个 ACK 到达 server，server 如果很久没有收到 ACK，会重发 ACK+FIN。

  [1]: http://ww4.sinaimg.cn/large/77691203gw1fb823efip1j20fm0m9jvo.jpg
  [2]: http://ww3.sinaimg.cn/large/77691203gw1fb8229bgc2j20ob0r40ut.jpg

