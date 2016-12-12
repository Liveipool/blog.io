---
layout: post
title: TCP、UDP及端口的一些概念总结。
date: 2016-09-13
categories: blog
tags: [网络基础]
description: 网络基础
---


# TCP、UDP及端口的一些概念总结。

尽管位于传输层的协议TCP和UDP均使用相同的网络层协议IP，但这两种传输层协议却为应用层提供了完全不同的服务：**TCP提供一种面向连接的、可靠的数据流服务，而UDP则提供一种面向独立数据报的高效传输服务。**  

(1) TCP 传输控制协议
传输控制协议（TCP：Transfer Control Protocol）是一种面向连接的传输层协议，可为两台不同主机上的应用程序提供可靠的数据流连接。所谓“面向连接”，意味着两个使用TCP通信的应用程序在交换数据之前，必须先建立一个TCP连接，待通信结束后须关闭该连接。这一过程与电话通信相似：先拨号振铃，等待对方摘机应答后再表明身份，通话完毕后挂机。  

TCP执行的任务包括把应用层传来的数据分解为合适的数据段交给网络层，确认接收到的数据分组，设置发送最后确认分组的超时时间等，为应用层屏蔽了实现端到端可靠通信的细节。TCP可保证数据从连接的一端送到另一端时仍能保持原来发送时的次序，否则将一个传输错误。TCP为需要可靠通信的应用程序提供了一种点对点信道，在因特网上常见的FTP、Telnet、SMTP等大多数应用层协议都建立在TCP的基础上。  

(2) UDP 用户数据报协议
与TCP协议不同的是，用户数据报协议（UDP：User Datagram Protocol）不是基于连接的，不需要先建立连接，也没有组装和重传请求的功能，而是为应用层提供一种非常简单、高效的传输服务。UDP从一个应用程序向另一应用程序发送独立的数据报，但**并不保证这些数据报一定能到达另一方，并且这些数据报的传输次序无保障，后发送的数据报可能先到达目的地。**   使用UDP协议时，任何必需的可靠性都须由应用层自己提供，UDP适用于对通信可靠性要求低且对通信性能要求高的应用。  

虽然UDP不如TCP那样常用，但UDP因其固有的特点而在某些应用场合可更好地发挥特长。例如在一个TCP连接中仅允许两方参与通信，故广播和多播方式不能用于TCP协议，但UDP却能有效地支持广播和多播通信；又如，对一个播发时间的服务程序而言，重发丢失的数据已毫无意义，因为知道数据丢失后原来的时间已经过去。注意在适合UDP协议的各种应用中，**要么是因为TCP协议提供的可靠性服务是多余的，要么由应用程序自己实现了所需的可靠性。**  

(3) 端口
由于现代计算机大多运行多任务操作系统，故一台主机上可能同时运行多个应用程序进程，并且一个进程还可能使用多个不同的连接，因而仅用主机名或IP地址无法惟一地标识数据包的源或目标。端口为标识参与通信的主机、进程和连接提供了一种统一的、惟一的方法。  

因特网上数据的传输目标由主机和端口号组成，例如UDP这类基于数据报通信的每一数据报中均含有端口号信息。主机可由32位的IP地址标识（假设采用
IPv4），端口则采用16位数字标识，故端口号的取值范围为0-65535。编号为0-1023的端口保留给系统服务使用，包括HTTP、FTP、Telnet、SMTP等常见服务。   
**我们通常只自定义4位数的端口号是因为大多数TCP/IP实现给临时端口分配1024~5000之间的端口号。**  
![](http://upload-images.jianshu.io/upload_images/3001083-dd243fd77c821f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)