```
--一个restful接口，背后会调用多少个微服务？


基建这里花时间的能细聊下。背景是我们在生产环境下使用node服务经验不够，目前还是一些简单的文档工程和cms工程，用最普通的forever部署

容器化部署对服务器有哪些要求？

node ---ci--> docker img---> k8s/交付

dockerFile描述基础环境设施，代替serviceAdmin提供的机器

严选使用了容器化部署了么？ 非容器化部署是不是更新npm依赖会麻烦一些？  npm包的安全性，有一些额外保证措施么？
费用？

有一个问题我有点疑惑，如果说koa是node的趋势， 为什么egg之后出来的nest反而是基于express呢
我看到是express更适合spring风格的范式？

当然node本身也不止web应用框架这些问题这么狭窄，就是从web应用框架的角度， express貌似还是活得很好

http性能真的没影响吗？ 现在对client和对server分别是http2/S还是http1？ 

http直接调用可以走ip么？这样会不会省了dns更快？我们内部服务器ip是固定的么？

spring cloud推荐（默认，可以改成grpc？）各个服务之间使用http通信？

使用rpc框架如dubbojs需要关心zookeeper这样的服务注册中心么？

rpc调用中，鉴权信息如何传递？也是封装为参数么？
https调用的话，鉴权信息是按原样放在header中，还是提出来做为参数？提出做为参数，不保持header格式。这里有什么考量么

黄天立说，serverless、service mesh之类的，拿nest来搞都是很合适的？ koa或基于koa的egg相对没那么合适么？
他们函数粒度可以拆的比较细的？还可以拆开来部署的
那是说，koa的洋葱模在这里会造成某种劣势么？ 微服务力度不够细？ 中间件的耦合么

grahql有了bff还有必要么

基建：
1. 脚手架
2. 日志平台（王英）
3. 应用监控+业务监控（于志超）shimmer grafana kibana
4. 分布式链路跟踪（付超） 对应的消息发给kafaka   a、服务关系图；b、
5. CI/CD（杨波） 发布回滚
6. 配置中心 （吴子房） 开源阿波罗  热更新配置
7. 服务注册+发现 cousoul+nginx
8. ssr、BFF、fullstack--技术工具、内部管理系统、工作台

```

## RPC vs HTTP
```
--RPC vs HTTP? http restful 易读、灵活、低耦合，这些优点我能明白
但RPC高效低延迟这个性能指标相信是很多微服务梦寐以求的，如果一个请求所需要的服务是3个以上并且各个服务业务逻辑复杂，甚至涉及众多IO操作，那是不是RPC的方式更为保障。
在我印象中服务与服务的调用如果要用http方式的话，满足条件：远距离的第三方服务；低频服务。
或者说spring cloud 默认使用http restful（至少我查阅的资料全是http），而提供RPC方案？
— forfuns ·

￼￼￼HTTP Restful本身轻量，易用，适用性强，可以很容易的跨语言，跨平台，或者与已有系统交互，毕竟HTTP现在没有不支持的。Spring可以整合其他的RPC方案，比如各种MQ，Hessian，Thrift，都可以。但是各类RPC协议本身有各自的使用范围和编码要求，这些会对交互两端的代码形成约束，所以应该根据自身实际情况去选择。至于各类整合方案，应该很多，可以带着具体的RPC协议去搜
— ivanna 


感谢你的回答，目前我的理解和你是一样，但我没有找到RPC相关的资料，不过我的疑惑主要是相比于这种灵活性，大家更愿意牺牲性能？还是说牺牲的这部分性能其实并不多？
— forfuns · 2017年08月25日


性能方面，RPC肯定是占优的，但至于这种优势有多大意义，涉及到的情况就很多了，比如并发，数据量等等。而其他一些方面，比如安全性，开发成本，适用性等方面，可能http优势更大。建议对外的或者与其他系统的，除非有非常大的性能要求，一般均使用http，内部自建的，比如微服务架构，选一种自己能够handle的RPC，之前我们用protobuf，但是在开发，调试上都不是很方便，目前改hession了，正在考虑json rpc，其实有的时候，性能这东西优先级不是很高，可能快速迭代，上线更重要一点
— ivanna · 2017年08月25日


回复 ivanna：
"性能这东西优先级不是很高" 为啥？在高并发大流量下，性能还是很重要的吧
 — 扬子将庞 · 1月2日

1 
回复 扬子将庞：
前提是要有高并发和大流量，大部分的功能往往并没有这方面的需求，其次spring cloud提倡服务拆分，也分散了并发和流量，加上MQ削峰，镜像部署扩容和serverless这些技术，再平衡开发难度，灵活性，迭代等需求，所以前期设计部分性能需求是可以绕过或者降低优先级的，当然需要考虑好后续扩展，替换的可行以及成本，一方面不要过度设计，增加前期不必要的复杂度，另一方面要周全，有应急备案。
— ivanna · 1月7日
 
```

## YKT实现

```
1. 应用框架
2. SSR
3. 微服务通讯
4. 链路追踪
5. 性能监控
6. 日志

遗留问题：
docker部署，k8s部署
eggjs与next的集成问题
eggjs基于koa，koa稳定中间件没有express多的问题


应用框架
	a. egg - 基于koa2封装
	b. nestjs - 基于express封装	
	c. 基于koa2自己封装
	d. 基于express自己封装	

SSR
	React - next (与eggjs集成不便)
	Vue - nuxt(与eggjs集成不便)
	React-beidou


微服务通讯
	dubbo-rest-zk
	dubbo-jsonrpc-zk
	springcloud-rest-eureka
	-----------------
	dubbo-hessain-zk  
	kaola-dubbo.js  -  cluster-client、egg-logger
	apache-dubbo2.js
	node-zookeeper-dubbo

链路追踪
	zipkin
	zipkin-instrumentation-express
	zipkin-instrumentation-request

性能监控
	alinode
	哨兵+探针

日志
	EOK 杭研

部署
	pm2? egg-script? docker?k8s?
```