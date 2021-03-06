---
layout: post
title: 延迟与宽带
date: 2017-03-16
categories: blog
description: 基础
---

以下为《web性能权威指南》上的一些总结：        

# 延迟与宽带         
 - **延迟**：分组从信息源发送到目的地所需的时间。         
 - **带宽**：逻辑或物理通信来路经最大的吞吐量。         
 - **分组（packet）**：大多数计算机网络都不能连续传送任意长的数据，所以实际上网络系统把数据分割成小块，然后逐块的发送，这种小块就称为分组。         
![1.1.png](http://upload-images.jianshu.io/upload_images/3001083-602398bf33710c62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

### 延迟的构成         
 - 延迟是**消息**（message）或**分组**（packet）从起点到终点经历的时间。         
 - **路由器**：负责在客户端和服务器之间转发消息的设备。         
 - **处理延迟**：处理分组首部、检查位错误及确定分组目标所需的时间。         
 - **排队延迟**：分组等待处理的时间。         
 - **传输延迟**：把分组中的所有比特转移到链路中需要的事件，是消息长度和链路速率的函数。         
 - **传播延迟**：消息从发送端到接收端需要的时间，是信号传播距离和速度的函数。         
 - 以上四点就是客户端到服务器的总延迟时间，传输延迟由传输链路的速率决定，而传播速度通常不超过光速。路途中经过的路由器越多，处理、排队、传输延迟就越多。         

### 光速与传播延迟         

> 爱因斯坦在他的狭义相对论里所说的，光速是所有能量、物质和信息运动所能达到的最高速度，这个结论给网络分组的传播速度设定了上限。         

但是在实际中，由于网络中的分组是通过**铜线、光纤等介质**传播的，这些介质会导致传播速度变慢。光速与分组介质中传播速度之比，叫做该介质的折射率。         
至今，折射率最小已经降低到了1.4、1.5左右。         

在软件交互中，100～200ms的延迟，都会让人感觉到拖拉，更别说延迟再往上增加的时候。认真对待网络延迟是非常有必要的。         

**CDN**（Content Delivery Network，内容分发网络）服务的用途很多，但最重要的就是通过把内容部署在全球各地。让用户从最近的服务器加载内容，大幅降低传播分组的事件。         

### 延迟的最后一公里         
![1.2.png](http://upload-images.jianshu.io/upload_images/3001083-e0268f535b1b7395.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

最后一公里的延迟与提供商、部署方法、网络拓扑，甚至一天中的哪个时段都有很大关系。作为最终用户，如果想要提高上网的速度，那选择延迟最短的ISP是最关键的。         

> 大多数网站性能的瓶颈都是延迟，而不是带宽。         

### 网络核心的带宽         
**光纤**就是一根“光导管”，专门用来从一端向另一端传送光信号。**金属线**则用于传送电信号，但信号损失、电磁干扰较大，同时维护成本也较高。这两种线路我们的数据分组很可能都会经过，但一般长距离的分组传输都是通过**光纤**完成的。         

通过波分复用（WDM）技术，光纤可以同时传输很多不同波长的光，因而具有明显的带宽优势。一条光纤连接的总带宽，等于每个信道的数据传输速率乘以可复用的信道数。         

### 网络边缘的带宽         
构成因特网核心数据路径的骨干或光纤连接，每秒能够移动数百比特信息。然而，网络边缘的容量就小得多了，而且很大程度上取决于部署技术，比如拨号连接、电缆等。用户可用带宽取决于客户端与目标服务器间最低容量连接。         

虽然与ISP的高带宽连接是必要的，但这个高带宽并不能保证端到端的传输速度。由于请求密集、硬件故障、网络攻击，以及其他很多原因，网络的某个中间节点随时都有可能发生拥塞。**吞吐量和延迟波动大是因特网固有的特点。**         
