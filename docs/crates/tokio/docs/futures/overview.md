# 概述

`Future` 对象，在前文中曾多次提到的，是用来管理异步逻辑的基本构件，也是 Tokio 所使用的底层异步抽象。

一个 future 值代表了一个异步计算的完成。通常来讲，future 会因系统的其他部分产生的事件而完成。我们已经通过基本I/O看过一些具体的案例，然而你可以用一个 future 去表示更多可能的事件，比如：

* **一次数据库查询**，当查询结束时，future 对象被完成，它的值就是查询的结果.

* **一次RPC（远程过程调用）** 。 当远程服务器响应时，future 对象被完成，它的值就是服务器的相应。

* **一次超时**. 当给定时间耗尽时，future 对象被完成，它的值为 `()`。

* **一个耗时较长的CPU密集型任务**，运行在一个线程池中。当任务结束时，future 对象被完成，它的值就是任务的返回值。

* **从一个套接字中读取若干字节**. 当这若干字节准备完毕，future 对象被完成——这取决于你的缓冲策略，这些字节可能被直接返回，或者已函数副作用的方式被写入到预先分配好的缓冲区中。

Tokio 的应用程序是以 future 对象为单位进行构造的，Tokio 掌握这些 future 对象并驱动它们执行。
