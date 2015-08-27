---
layout: post
title:  "dubbo问题"
date:   2015-08-28 19:22:54
categories: dubbo
---
现在遇到了一个问题，我有一个服务器群组，里面有一个服务ServiceA（举例）作为这个服务的调用方，给服务器群组里面的部分组做远程RPC调用，但是在本地的环境中，经常出现，当服务启动的时候，报了com.alibaba.dubbo.remoting.RemotingException这个错误，具体的如下：
	```com.alibaba.dubbo.remoting.RemotingException: client(url: dubbo://10.15.1.84:20880/com.lvmama.vst.newsearch.lucene.search.service.ComLuceneSearchService?anyhost=true&application=vst_search&check=false&codec=dubbo&default.check=false&default.retries=0&default.timeout=120000&dubbo=2.5.3&heartbeat=60000&interface=com.lvmama.vst.newsearch.lucene.search.service.ComLuceneSearchService&methods=searchHotelAutoComplete,searchDest,searchAll,search,searchHotelAutoCompleteByKeyword,searchHotelAutoParentIdMappingDistrictId&monitor=dubbo%3A%2F%2F192.168.0.107%3A7070%3Fdubbo%3D2.5.3%26interface%3Dcom.alibaba.dubbo.monitor.MonitorService%26pid%3D2972%26timestamp%3D1440668668251&pid=2972&side=consumer&timestamp=1440668668243) failed to connect to server /10.15.1.84:20880, error message is:Connection refused: no further information
	at com.alibaba.dubbo.remoting.transport.netty.NettyClient.doConnect(NettyClient.java:124)
	at com.alibaba.dubbo.remoting.transport.AbstractClient.connect(AbstractClient.java:280)
	at com.alibaba.dubbo.remoting.transport.AbstractClient$1.run(AbstractClient.java:145)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:439)
	at java.util.concurrent.FutureTask$Sync.innerRunAndReset(FutureTask.java:317)
	at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:150)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$101(ScheduledThreadPoolExecutor.java:98)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.runPeriodic(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:204)
	at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:895)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:918)
	at java.lang.Thread.run(Thread.java:662)
	Caused by: java.net.ConnectException: Connection refused: no further information
	at sun.nio.ch.SocketChannelImpl.checkConnect(Native Method)
	at sun.nio.ch.SocketChannelImpl.finishConnect(SocketChannelImpl.java:599)
	at org.jboss.netty.channel.socket.nio.NioClientSocketPipelineSink$Boss.connect(NioClientSocketPipelineSink.java:396)
	at org.jboss.netty.channel.socket.nio.NioClientSocketPipelineSink$Boss.processSelectedKeys(NioClientSocketPipelineSink.java:358)
	at org.jboss.netty.channel.socket.nio.NioClientSocketPipelineSink$Boss.run(NioClientSocketPipelineSink.java:274)
	... 3 more```

请问有谁遇到类似的本地问题吗？

解决方案
----
