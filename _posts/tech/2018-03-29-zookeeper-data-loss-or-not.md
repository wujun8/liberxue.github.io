---
layout: blog
background: black
background-image: https://cdn.pixabay.com/photo/2017/09/19/10/26/data-2764822_1280.jpg
title: Zookeeper 数据神出鬼没？
tags:
- zookeeper
- 数据
- 系统
published: false
---

#### 问题简述

在 Zookeeper 注册中心存储的数据不定时会丢失，不得不再次重新注册一次。

偶尔发生一两次就算了，但是后来是隔不久出现一次，不得不找找原因了。

#### 背景
[Zookeeper](https://zookeeper.apache.org/){:target="_blank"} 官网看看 WiKi，
有翻译好的推荐看下 [分布式服务框架 Zookeeper](https://www.ibm.com/developerworks/cn/opensource/os-cn-zookeeper/){:target="_blank"}

下面简单说下笔者的情况，系统由 Java 编写，直接引用了 Zookeeper 的 jar，同主应用程序进程一同
新开了线程启动 Zookeeper 服务，提供信息注册服务，数据持久化目录为 ```/tmp/zookeeper```

#### 解决思路

* 数据丢失一般发生在重启应用后，发现数据丢失，那么切入点就在于 shutdown 的过程
* 看了源码，在关闭前会做调用一个 ```cleanDB```