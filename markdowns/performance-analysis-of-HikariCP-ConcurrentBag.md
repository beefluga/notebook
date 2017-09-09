### 前言
ConcurrentBag是光连接池自己实现的并发池，不仅限于管理数据库连接对象，也可以通过扩展 `IBagStateListener` 实现其他类型对象的池化管理，类似commons-pool的GenericObjectPool。关于如何池化自定义对象，阅读这篇文章的同学可以自己先去考虑，后期我会考虑写一点关于这个主题的文章，而这篇文章的重点主要是分析ConcurrentBag的性能。

### 如何实现连接池
分析ConcurrentBag之前，我们先思考一下，如果让我们写一个连接池，怎么去实现呢？

- 需要一个容器保存连接对象
- 提供borrow和return两个借和还的API
- 借连接时可能需要阻塞。因为并发的进程大于容器中连接数量的那个时刻是没有可用连接的，所以需要等待至有可用连接。

如果只考虑这三个场景（事实上连接池远远不止这三点），我们最先想到能满足这种场景的数据结构是阻塞队列。阻塞队列的特性帮我们解决了借连接时可能会发生阻塞的场景。事实上大多数连接池的核心数据结构就是阻塞队列。