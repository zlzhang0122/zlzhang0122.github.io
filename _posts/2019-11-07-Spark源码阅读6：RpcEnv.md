RPC环境承担着Spark体系内几乎所有的内部及外部通信，RpcEnv抽象类是Spark RPC环境的通用表示，其中定义的setupEndpoint()方法用来注册一个RPC端点(RpcEndpoint)，
并返回其引用(RpcEndpointRef)。如果客户端想向一个RpcEndpoint发送消息，那么首先必须获取其对应RpcEndpoint的引用。其关系如下所示：
![RpcEnv](../assets/img/spark/rpc-env.png "RpcEnv")

由上图可知RpcEndpoint和RpcEndpointRef是RPC环境中的基础组件，其中RpcEndpoint是一个trait，其定义的方法有：
  * self()：获取当前RpcEndpoint对应的RpcEndpointRef。

  * receive()/receiveAndReply()：接收其它RpcEndpointRef发送的消息并处理，其中receiveAndReply()方法还会发送回复。

  * onError()：消息处理出现异常时调用。

  * onConnected()/onDisconnected()：当前RpcEndpoint建立或断开连接时调用。

  * onNetworkError()：RpcEndpoint的连接出现网络错误时调用。

  * onStart()/onStop()：RpcEndpoint初始化与关闭时调用。

  * stop()：停止当前RpcEndpoint。

其继承体系如下图所示：
![RpcEndpoint继承体系](../assets/img/spark/rpcendpoint.png "RpcEndpoint继承体系")

上图中可以看到先前出现过的RPC端点，如HeartbeatReceiver、MapOutputTrackerMasterEndpoint、BlockManagerMasterEndpoint等。此外，上图中的
ThreadSafeRpcEndpoint时直接继承自RpcEndpoint的trait，即它要求RPC端点对消息的处理时线程安全的也就是满足happens-before原则。