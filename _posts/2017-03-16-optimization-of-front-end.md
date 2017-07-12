---
layout: post
title: 常见的前端性能优化方法总结
date: 2017-03-16
categories: blog
description: JavaScript学习
---

# 常见的前端性能优化方法总结        
最近准备面试也经常看到前端优化的问题，下面来总结一下，可根据优化方向对优化方法进行划分：        

## 一、请求数量        
以下四种优化方式的核心思想都是减少HTTP请求数量。        

#### 1.合并脚本和样式表        
减少HTTP请求数量，减少延迟。        
        
#### 2.CSS Sprites        
CSSSprites在国内很多人叫css精灵或雪碧，是一种网页图片应用处理方式。它允许你将一个页面涉及到的所有零星图片都包含到一张大图中去。                
它加速的关键，**不是降低质量，而是减少个数**。传统切图讲究精细，图片规格越小越好，重量越小越好，其实规格大小无所谓，计算机统一都按byte计算。客户端每显示一张图片都会向服务器发送请求。所以，**图片越多请求次数越多，造成延迟的可能性也就越大。**        
CSS Sprites其实就是把网页中一些背景图片整合到一张图片文件中，再利用CSS的“background-image”，“background- repeat”，“background-position”的组合进行背景定位，background-position可以用数字精确的定位出背景图片的位置。        

优点：        
1. 减少网页的http请求。        
2. 减少图片的字节，曾经比较过多次3张图片合并成1张图片的字节总是小于这3张图片的字节总和。        

缺点：        
1. 在图片合并的时候，你要把多张图片有序的合理的合并成一张图片，还要留好足够的空间，防止板块内出现不必要的背景；这些还好，最痛苦的是在宽屏，高分辨率的屏幕下的自适应页面，你的图片如果不够宽，很容易出现背景断裂；        
2. 开发的时候要想将多张图片合并成一张比较麻烦，要通过photoshop或者样式生成工具来合并。        
3. CSS Sprites在维护的时候比较麻烦，如果页面背景有少许改动，一般就要改这张合并的图片，无需改的地方最好不要动，这样避免改动更多的css，如果在原来的地方放不下，又只能（最好）往下加图片，这样图片的字节就增加了，还要改动css。        

#### 3.拆分初始化负载        
将页面一开始加载时不需要执行的资源从所有资源中分离出来，等到需要的时候再加载。        

#### 4.根据域名划分内容        
减少每个域上的请求数量。浏览器一般对同一个域的下载连接数有所限制，按照域名划分下载内容可以浏览器增大并行下载连接，但是注意控制域名使用在2-4个之间，不然dns查询也是个问题。        

## 二、请求带宽        
以下几种优化主要是希望在有限的带宽中能够尽快地传输文件。        
        
#### 5.开启GZip        
服务器中的这个功能将输出到用户浏览器的数据进行压缩的处理，这样就会减小通过网络传输的数据量，提高浏览的速度。        

#### 6.精简JavaScript        
相当于也是在压缩代码        

#### 7.移除重复脚本        
同上        

#### 8.图像优化        
降低图像的大小，减少传输时间。        

## 三、缓存利用        

#### 9.使用CDN（内容分发网络 content delivery network）        
把网站内容分散到多个、处于不同地域位置的服务器上可以加快下载速度。        

#### 10.使用外部JavaScript和CSS        
外部文件可缓存，易维护，必要时无须一次全部加载。        

#### 11.添加Expires头        
Expires存储的是一个用来控制缓存失效的日期。当浏览器看到响应中有一个Expires头时，它会和相应的组件一起保存到其缓存中，只要组件没有过期，浏览器就会使用缓存版本而不会进行任何的HTTP请求。Expires设置的日期格式必须为GMT（格林尼治标准时间）。        

#### 12.减少DNS查找        
无须多说        

#### 13.配置ETag        
实体标签(EntityTag)是唯一标识了一个组件的一个特定版本的字符串，是web服务器用于确认缓存组件的有效性的一种机制，通常可以使用组件的某些属性来构造它。        

条件GET请求：（Last-Modified和If-Modified-Since）        
浏览器下载组件的时候，会将它们存储到浏览器缓存中。如果需要再次获取相同的组件，浏览器将检查组件的缓存时间，假如已经过期，那么浏览器将发送一个条件GET请求到服务器，服务器判断缓存还有效，则发送一个304响应，告诉浏览器可以重用缓存组件。        
那么服务器是根据什么判断缓存是否还有效呢?答案有两种方式，一种是前面提到的ETag，另一种是根据最新修改时间。先来看看最新修改时间。        

为什么要引入ETag：        
**ETag主要是为了解决条件GET中Last-Modified无法解决的一些问题：**        
1. 一些文件也许会周期性的更改，但是他的内容并不改变(仅仅改变的修改时间)，这个时候我们并不希望客户端认为这个文件被修改了，而重新GET;        
2. 某些文件修改非常频繁，比如在秒以下的时间内进行修改，(比方说1s内修改了N次)，If-Modified-Since能检查到的粒度是s级的，这种修改无法判断(或者说UNIX记录MTIME只能精确到秒);        
3. 某些服务器不能精确的得到文件的最后修改时间。        
        
#### 14.使Ajax可缓存        
针对页面中主动的Ajax请求返回的数据要缓存到本地，当然这个是针对短期内不会变化的数据。如果不确定数据变化周期的话，可以增加一个修改标识的判断，如处理过程中会给一些Ajax请求返回的数据增加一个MD5值的判断，每次请求会判断当前MD5是否变化，如果变化了取最新的数据，如果不变化，则不变.        

## 四、页面结构        

#### 15.将样式表放在顶部，将脚本放在底部，尽早刷新文档的输出        
样式表放在头部可以让用户尽早看到良好的页面。        

## 五、代码校验        
使用代码检验可以避免CSS表达式和重定向。        

#### 16.避免CSS表达式        
因为页面样式可能会不断变化，因此表达式可能在不经意间执行了成千上万次，因此要尽量避免css表达式。（为了确保有效性，CSS 表达式会进行频繁的求值，到底有多频繁？就是在你改变窗口大小，滚动页面甚至移动鼠标都会触发表达式进行求值，如此频繁的求值以至于浏览器的性能收到严重的影响；）        

#### 17.避免重定向        
重定向用于将用户从一个URL重新路由到另一个URL。        

常用重定向的类型：        
 - 301：永久重定向，主要用于当网站的域名发生变更之后，告诉搜索引擎域名已经变更了，应该把旧域名的的数据和链接数转移到新域名下，从而不会让网站的排名因域名变更而受到影响。        
 - 302：临时重定向，主要实现post请求后告知浏览器转移到新的URL。        
 - 304：Not Modified，主要用于当浏览器在其缓存中保留了组件的一个副本，同时组件已经过期了，这是浏览器就会生成一个条件GET请求，如果服务器的组件并没有修改过，则会返回304状态码，同时不携带主体，告知浏览器可以重用这个副本，减少响应大小。        

重定向如何损伤性能：        
当页面发生了重定向，就会延迟整个HTML文档的传输。在HTML文档到达之前，页面中不会呈现任何东西，也没有任何组件会被下载。        

来看一个实际例子：对于ASP.NET webform开发来说，对于新手很容易犯一个错误，就是把页面的连接写成服务器控件后台代码里，例如用一个Button控件，在它的后台click事件中写上：Response.Redirect("")；然而这个Button的作用只是转移URL，这是非常低效的做法，因为点击Button后，先发送一个Post请求给服务器，服务器处理Response.Redirect("")后就发送一个302响应给浏览器，浏览器再根据响应的URL发送GET请求。正确的做法应该是在html页面直接使用a标签做链接，这样就避免了多余的post和重定向。        