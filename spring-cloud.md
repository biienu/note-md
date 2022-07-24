#  一、SpringCloud 相关组件

![image-20220717202101922](D:/ProgramFiles/typora/typora-images/image-20220717202101922.png)

***

# 二、服务注册中心

## 1.Eureka服务注册中心

**Eureka两个组件 ：Eureka Server 和 Eureka Client**:

![image-20220717202229977](D:/ProgramFiles/typora/typora-images/image-20220717202229977.png)

***

![](D:/ProgramFiles/typora/typora-images/image-20220717204159949.png)

***

**注册中心服务**：

1. pom.xml文件加入依赖
   
   ![image-20220717202541250](D:/ProgramFiles/typora/typora-images/image-20220717202541250.png)

   +++
   
2. application.yaml
   
   ![image-20220717202509580](D:/ProgramFiles/typora/typora-images/image-20220717202509580.png)

3. 主启动类
   
   ![image-20220717202708984](D:/ProgramFiles/typora/typora-images/image-20220717202708984.png)

**Provider服务**(Eureka Client):

1. pom.xml文件加入依赖

   ![image-20220717203116896](D:/ProgramFiles/typora/typora-images/image-20220717203116896.png)

2. application.yaml配置文件

   ![](D:/ProgramFiles/typora/typora-images/image-20220717203953039.png)

3. 主启动

   ![image-20220717203724425](D:/ProgramFiles/typora/typora-images/image-20220717203724425.png)



## 2.Eureka集群原理及搭建

**Eureka集群原理**

![image-20220717204939422](D:/ProgramFiles/typora/typora-images/image-20220717204939422.png)

+++

![image-20220717210420644](D:/ProgramFiles/typora/typora-images/image-20220717210420644.png)

***



**服务注册中心application.yaml配置文件更改:**

![image-20220717210905378](D:/ProgramFiles/typora/typora-images/image-20220717210905378.png)

![image-20220717211131387](D:/ProgramFiles/typora/typora-images/image-20220717211131387.png)

****

**服务客户端配置文件修改**:

![image-20220717211653280](D:/ProgramFiles/typora/typora-images/image-20220717211653280.png)

**服务启动顺序：先启动EurekaService服务再启动EurekaClient服务**

***

**EurekaClient集群搭建:**

![image-20220717214029234](D:/ProgramFiles/typora/typora-images/image-20220717214029234.png)

***

***

![image-20220717215240284](D:/ProgramFiles/typora/typora-images/image-20220717215240284.png)



进行服务调用时，需要开启**负载均衡**

![](D:/ProgramFiles/typora/typora-images/image-20220717220222102.png)

***

服务调用地址， 写**服务名**

![image-20220717220400377](D:/ProgramFiles/typora/typora-images/image-20220717220400377.png)

***



**服务发现Discovery:**

1. 启动类上开启服务发现

   ![image-20220717222227798](D:/ProgramFiles/typora/typora-images/image-20220717222227798.png)

2. 注入`DiscoveryClient`对象。

   ```java
   @Resource
   private DiscoveryClient discoveryClient;
   //通过discoveryClient对象可以获取注册在注册中心中的服务的相关信息
   ```

   ![image-20220717222749310](D:/ProgramFiles/typora/typora-images/image-20220717222749310.png)

3. 自我保护机制

   ![image-20220717225622326](D:/ProgramFiles/typora/typora-images/image-20220717225622326.png)

4. 关闭自我保护机制

   ![image-20220717230323957](D:/ProgramFiles/typora/typora-images/image-20220717230323957.png)

***



## 3.Zookeeper服务注册与发现

[csdn:  zookeeper服务注册与发现](https://blog.csdn.net/qq_51134950/article/details/123415777)



## 5. Consul服务注册与发现

[csdn: consul服务注册与发现](https://blog.csdn.net/qq_51134950/article/details/123431856)



# 三、服务调用 

## 1. Ribbon

**什么是`Ribbon`?**

![image-20220717231051413](D:/ProgramFiles/typora/typora-images/image-20220717231051413.png)

***

**什么是负载均衡？Ribbon和Nginx的区别?**

![image-20220718091626959](D:/ProgramFiles/typora/typora-images/image-20220718091626959.png)

***

**高版本spring-cloud-starter-netflix-eureka-client**依赖中会包含**Ribbon依赖**

**Ribbon和RestTemplate结合完成负载均衡和服务调用**

![image-20220718092930930](D:/ProgramFiles/typora/typora-images/image-20220718092930930.png)



**Ribbon自带的几种负载均衡算法**：

![image-20220718093253362](D:/ProgramFiles/typora/typora-images/image-20220718093253362.png)

***

**自定义修改Ribbon的负载均衡算法**:

1. 自定义配置类注意事项

![image-20220718094227658](D:/ProgramFiles/typora/typora-images/image-20220718094227658.png)

2. 自定义的配置类

   ![image-20220718094605361](D:/ProgramFiles/typora/typora-images/image-20220718094605361.png)

3. 主启动添加`@RibbonClient`注解

   ![image-20220718094918902](D:/ProgramFiles/typora/typora-images/image-20220718094918902.png)


+++

## 2. OpenFeign

**使用方式？**

只需创建一个接口，接口上添加注解即可。

**Feign能干什么？**

![image-20220718194145766](D:/ProgramFiles/typora/typora-images/image-20220718194145766.png)

***

**Feign和OpenFeign的区别?**

![image-20220718194309966](D:/ProgramFiles/typora/typora-images/image-20220718194309966.png)

***

**OpenFeign使用步骤**：

服务消费方操作：

1. pom.xml文件添加依赖

   ![image-20220718194817511](D:/ProgramFiles/typora/typora-images/image-20220718194817511.png)

2. 主启动类上开启**OpenFeign**

   ![image-20220718200002808](D:/ProgramFiles/typora/typora-images/image-20220718200002808.png)

3. 业务类

   1. 服务提供方的业务接口：

      ![image-20220718200210143](D:/ProgramFiles/typora/typora-images/image-20220718200210143.png)

   2. 服务消费方所建的接口:

      ![](D:/ProgramFiles/typora/typora-images/image-20220718200553690.png)

4. Controller层

   **服务消费方调用服务提供方接口：**

   ![](D:/ProgramFiles/typora/typora-images/image-20220718201053457.png)

***



**OpenFeign超时控制 **

**OpenFeign-Ribbon，客户端一般默认等待 1 秒钟**：

服务调用方调用服务提供方的接口时，超出1秒钟就会报错。

**服务调用方（客户端）的`application.yaml`文件添加配置：**

![image-20220718211431074](D:/ProgramFiles/typora/typora-images/image-20220718211431074.png)

****



**OpenFeign日志增强**:

![image-20220718212244154](D:/ProgramFiles/typora/typora-images/image-20220718212244154.png)

***

**日志级别**：

![image-20220718212303437](D:/ProgramFiles/typora/typora-images/image-20220718212303437.png)

***

**增加日志bean**:

![image-20220718212420380](D:/ProgramFiles/typora/typora-images/image-20220718212420380.png)

***

**application.yaml**文件：

![image-20220718212553377](D:/ProgramFiles/typora/typora-images/image-20220718212553377.png)

***



# 四、服务降级、熔断、限流

**简单概念：**

**服务降级：fallback**

![image-20220719093957855](D:/ProgramFiles/typora/typora-images/image-20220719093957855.png)

***

**熔断：break**

类比保险丝达到最大服务访问后，直接拒绝访问，然后调用服务降级的方法返回友好提示。

**限流：flowlimit**

![image-20220719094331296](D:/ProgramFiles/typora/typora-images/image-20220719094331296.png)

***

![image-20220718213043080](D:/ProgramFiles/typora/typora-images/image-20220718213043080.png)



## 1. Hystrix

**Hystrix是什么？**

![image-20220718213134759](D:/ProgramFiles/typora/typora-images/image-20220718213134759.png)

***



# 五、服务网关Gateway

**什么SpringCloudGateway?**

![image-20220719141455932](D:/ProgramFiles/typora/typora-images/image-20220719141455932.png)

***

**Springcloud-gateway三个概念？**

1. **Route(路由)**

   ![image-20220719142307556](D:/ProgramFiles/typora/typora-images/image-20220719142307556.png)

2. **Predicate(断言)**

   ![image-20220719142401977](D:/ProgramFiles/typora/typora-images/image-20220719142401977.png)

3. **Filter(过滤)**

   ![image-20220719142448581](D:/ProgramFiles/typora/typora-images/image-20220719142448581.png)

***



**配置一个Gateway**:

操作步骤：

1. 建module，网关服务

2. 改pom.xml文件

   ![image-20220719145959161](D:/ProgramFiles/typora/typora-images/image-20220719145959161.png)

3. application.yaml文件

   ![](D:/ProgramFiles/typora/typora-images/image-20220719150524023.png)

4. 主启动类

   ![image-20220719150332188](D:/ProgramFiles/typora/typora-images/image-20220719150332188.png)

5. 访问时：

   ![image-20220719151038656](D:/ProgramFiles/typora/typora-images/image-20220719151038656.png)

   直接访问网关即可

   ***

**自定义配置网关（不使用yaml）**

![image-20220719152109668](D:/ProgramFiles/typora/typora-images/image-20220719152109668.png)

****

**通过微服务名实现动态路由**

![image-20220719152843838](D:/ProgramFiles/typora/typora-images/image-20220719152843838.png)



**springcloud-gateway 12种predicate**:

[springcloud-gateway 12种predicate](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#gateway-request-predicates-factories)

![image-20220719153802339](D:/ProgramFiles/typora/typora-images/image-20220719153802339.png)

***

**GatewayFilter Factories**:

[Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#gatewayfilter-factories)

![image-20220719154848312](D:/ProgramFiles/typora/typora-images/image-20220719154848312.png)

***



*example*:

![image-20220719203314420](D:/ProgramFiles/typora/typora-images/image-20220719203314420.png)

***

**自定义过滤器**

实现`GlobalFilter, Ordered`两个接口，

![image-20220719203801645](D:/ProgramFiles/typora/typora-images/image-20220719203801645.png)

***



# 六、服务配置



***



# 一、SpringCloudAlibaba



























































































