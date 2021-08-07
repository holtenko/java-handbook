- [\# RPC框架简单说下](#-rpc框架简单说下)
- [\# 说一下的 Dubbo 的工作原理/组成？](#-说一下的-dubbo-的工作原理组成)
- [\# 注册中心挂了 RPC 可以继续通信吗？](#-注册中心挂了-rpc-可以继续通信吗)
- [\# Dubbo 支持哪些序列化协议？说一下 Hessian 的数据结构？PB 知道吗？为什么 PB 的效率是最高的？](#-dubbo-支持哪些序列化协议说一下-hessian-的数据结构pb-知道吗为什么-pb-的效率是最高的)
- [\# Dubbo 负载均衡策略和高可用策略都有哪些？动态代理策略呢？](#-dubbo-负载均衡策略和高可用策略都有哪些动态代理策略呢)
- [\# Dubbo 的 SPI 思想是什么？](#-dubbo-的-spi-思想是什么)
- [\# Dubbo 和 thrift 有什么区别呢？](#-dubbo-和-thrift-有什么区别呢)

## \# RPC框架简单说下
- RPC框架的组成：
1. 服务提供者（Server）：对外提供后台服务，将自己的服务信息，注册到注册中心。
2. 注册中心（Registry）：用于服务端注册远程服务以及客户端发现服务。目前主要的注册中心可以借由 zookeeper，eureka，consul，etcd 等开源框架实现。
3. 服务消费者（Client）：从注册中心获取远程服务的注册信息，然后进行远程过程调用。

- RPC的工作流程：
1. 服务调用方（client）调用以本地调用方式调用服务；
2. client stub接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体；在Java里就是序列化的过程
3. client stub找到服务地址，并将消息通过网络发送到服务端；
4. server stub收到消息后进行解码,在Java里就是反序列化的过程；
5. server stub根据解码结果调用本地的服务；
6. 本地服务执行处理逻辑；
7. 本地服务将结果返回给server stub；
8. server stub将返回结果打包成消息，Java里的序列化；
9. server stub将打包后的消息通过网络并发送至消费方
10. client stub接收到消息，并进行解码, Java里的反序列化；
11. 服务调用方（client）得到最终结果。

## \# 说一下的 Dubbo 的工作原理/组成？
1. service 层，接口层，给服务提供者和消费者来实现的
2. config 层，配置层，主要是对 dubbo 进行各种配置的
3. proxy 层，服务代理层，无论是 consumer 还是 provider，dubbo 都会给你生成代理，代理之间进行网络通信
4. registry 层，服务注册层，负责服务的注册与发现
5. cluster 层，集群层，封装多个服务提供者的路由以及负载均衡，将多个实例组合成一个服务
6. monitor 层，监控层，对 rpc 接口的调用次数和调用时间进行监控
7. protocal 层，远程调用层，封装 rpc 调用
8. exchange 层，信息交换层，封装请求响应模式，同步转异步
9. transport 层，网络传输层，抽象 mina 和 netty 为统一接口
10. serialize 层，数据序列化层

## \# 注册中心挂了 RPC 可以继续通信吗？
可以，因为刚开始初始化的时候，消费者会将提供者的地址等信息拉取到本地缓存，所以注册中心挂了可以继续通信。

## \# Dubbo 支持哪些序列化协议？说一下 Hessian 的数据结构？PB 知道吗？为什么 PB 的效率是最高的？
- dubbo 支持 hession、Java 二进制序列化、json、SOAP 文本序列化多种序列化协议。但是 hessian 是其默认的序列化协议。
- Hessian 的对象序列化机制有 8 种原始类型：原始二进制数据、boolean、64-bit date（64 位毫秒值的日期）、64-bit double、32-bit int、64-bit long、null、UTF-8 编码的 string，另外还包括 3 种递归类型：list for lists and arrays、map for maps and dictionaries、object for objects，还有一种特殊的类型：ref：用来表示对共享对象的引用。
- Protocal Buffer 其实是 Google 出品的一种轻量并且高效的结构化数据存储格式，性能比 JSON、XML 要高很多。
- PB 之所以性能如此好，主要得益于两个：第一，它使用 proto 编译器，自动进行序列化和反序列化，速度非常快，应该比 XML 和 JSON 快上了 20~100 倍；第二，它的数据压缩效果好，就是说它序列化后的数据量体积小。因为体积小，传输起来带宽和速度上会有优化。

## \# Dubbo 负载均衡策略和高可用策略都有哪些？动态代理策略呢？
负载均衡策略
1. RandomLoadBalance
2. RoundRobinLoadBalance
3. LeastActiveLoadBalance
4. ConsistentHashLoadBalance

高可用策略
1. Failover Cluster 模式：失败自动切换，自动重试其他机器；
2. Failfast Cluster 模式：一次调用失败就立即失败，常见于非幂等性的写操作；
3. Failsafe Cluster 模式：出现异常时忽略掉，常用于不重要的接口调用；
4. Failback Cluster 模式：失败了后台自动记录请求，然后定时重发，比较适合于写消息队列这种；
5. Forking Cluster 模式：并行调用多个 provider，只要一个成功就立即返回。常用于实时性要求比较高的读操作，但是会浪费更多的服务资源，可通过 forks="2" 来设置最大并行数；
6. Broadcast Cluster 模式：逐个调用所有的 provider。任何一个 provider 出错则报错（从 2.1.0 版本开始支持）；

动态代理策略：默认使用 javassist 动态字节码生成，创建代理类。但是可以通过 spi 扩展机制配置自己的动态代理策略。

## \# Dubbo 的 SPI 思想是什么？
SPI 就是 service provider interface ，说白了是什么意思呢，比如你有个接口，现在这个接口有 3 个实现类，那么在系统运行的时候对这个接口到底选择哪个实现类呢？这就需要 spi 了，需要根据指定的配置或者是默认的配置，去找到对应的实现类加载进来，然后用这个实现类的实例对象。

## \# Dubbo 和 thrift 有什么区别呢？
1. Thrift：通过IDL（接口定义语言）实现，Thrift是支持跨语言的，而要实现跨语言机制，就需要一种中间语言来完成，那就是IDL。首先定义好了IDL之后（即定义好了一个service），由于server和Client都需要持有这个与之语言相对应的服务接口，那就需要thrift来将IDL编译成与之语言相对应的接口类，比如server端是java，client端是C#，则server端需要编译成java类，client编译成C#类。然后server端负责接口的具体实现，client只需要持有这个对象接口，进行调用即可，然后通过网络通信传给server。server负责解析client传递过来的数据，由于服务对象接口统一为IDL（thrift格式），所以统一了解析形式，只和本地的开发语言有关。
2. Dubbo：通过java反射和动态代理实现这一功能，由于只支持Java，所以Client和Server端开发语言机制一样，所以它们能够通过spring框架进行依赖，server端一般将对象方法接口注册发布到Zookeper中，然后Client可以直接从Zookeper中获取server端发布的对象方法接口，这样Client可以通过Spring进行自动装配获得server端发布的对象方法服务接口。
RPC和HTTP的关系