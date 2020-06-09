### RPC与RESTFUL调用

​		什么是RPC呢？百度百科给出的解释是这样的：“RPC（Remote Procedure Call Protocol）——远程过程调用协议，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议”。

#### 1.既REST ，何 RPC ？

​		在 OpenStack 里的进程间通信方式主要有两种，一种是基于HTTP协议的RESTFul API方式，另一种则是RPC调用。那么这两种方式在应用场景上有何区别呢？有使用经验的人，就会知道：

-   前者（RESTful）主要**用于各组件之间的通信**（如nova与glance的通信），或者说用于组件对外提供调用接口。
-   后者（RPC）则用于同一组件中**各个不同模块之间的通信**（如nova组件中nova-compute与nova-scheduler的通信）

