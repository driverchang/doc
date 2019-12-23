###### springcloud学习笔记

1. eureka是一个高可用的组件，它没有后端缓存，每一个实例注册之后需要向注册中心发送信息（因此可以在内存中完成），默认情况下eureka server也是一个eureka client，必须在application.yml中进行说明从而进行区分。

2.  Springcloud两种服务调用方式 

   - ribbon + restTemplate ：ribbon先向注册中心注册，然后通过restTemplate来调用相应的服务
   - feign：是一个声明式的伪Http客户端，通过创建接口并注解来消费服务。通过@ FeignClient（“服务名”），来指定调用哪个服务

3. 服务故障的“雪崩”效应：由于网络或者自身原因导致单个服务出现问题，调用这个服务就会出现线程阻塞，当有大量请求涌入，servlet容器的线程资源会被消耗完，导致服务瘫痪。服务与服务有依赖性，故障会传播对整个服务系统造成灾难性的后果。断路器就是解决这类问题。具体实现：

   - ribbon中使用断路器：在调用服务的代码前加上 @HystrixCommand(fallbackMethod = “method”) “method"为具体的方法。例如

     ```
     @HystrixCommand(fallbackMethod = “method”)
     public String method(String name){
     	return “error”;
     }
     ```

   - 在feign中，feign自带了断路器只要在配置文件中打开它就行了打开方式：

     ```
     feign.hystrix.enable = true
     ```

4.  Zuul的主要功能是路由转发和过滤器，路由的作用：根据路由配置中的path将请求转发给不同的服务。zuulFilter的几个常用函数的功能：

   -  filterType：还回一个字符串代表过滤器的类型：
     - pre 路由之前
     - routing 路由中
     - post 路由之后
     - error 发送错误调用
   - filterOrder 过滤的顺序
   - shouldFilter 判断逻辑，是否过滤，true表示永远过滤
   - run 过滤器的具体逻辑

5. 配置中心组件spring cloud config（config server和config client）,可以将配置服务放在配置服务的内存（本地），也可以放在Git仓库。服务获取配置信息的流程：Config serve从Git仓库中读取配置信息，config client 从config server获取配置信息。

6. spring cloud bus（消息总线）将分布式的节点用轻量级的消息代理连接起来。它可以用于广播配置文件的更改或者服务之间的通讯，也可以用于监控。（广播：通知给所有的服务）。实现通知微服务框架的配置文件更改的流程：当存储在远程仓库或者本地的配置文件发生更改时，PC端用post向其中一个config client发送请求，config client会发送一个消息，由消息总线向其他服务传递，从而使整个服务集群都更新配置文件。