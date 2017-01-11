---
layout: default
title:  "TCP流量控制和拥塞控制"
author: itsmikej
category: TCP/IP
---

### 流量控制

利用窗口滑动窗口控制流量

![流量控制][1]

- 死锁问题
- 提升传输效率 - Nagle算法（缓存应用进程的数据，一并发送）
- Silly window syndrome（接收方缓存已满，但每次应用进程只读取一个字节

总的原则就是：发送方不要发送很小的报文，同时接收方也不要刚有一点空间就将窗口告知发送方

### 拥塞控制

 - 慢启动（slowstart） 
 - 拥塞避免（congestion avoidance） 
 - 快重传（fast retransmit）
 - 快恢复（fast recovery）

![拥塞控制][2]


  [1]: http://ww4.sinaimg.cn/large/0060lm7Tgw1fbmk0eryjqj30h60a1gnc.jpg
  [2]: http://ww4.sinaimg.cn/large/0060lm7Tgw1fbmk1za7ohj30lw0b73zz.jpg
