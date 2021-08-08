# Tech_Diagrams  技术图鉴
* 基于Draw.io 
* 个人收集和总结的所涉及到的一些全端技术流程分析,组织架构图
* 持续学习更新中~

<table>
    <tr>
        <td>Name</td> 
        <td>Category</td> 
        <td>Content</td> 
   </tr>
    <tr>
        <td rowspan="4">Architecture</td>    
        <td>Distributed system</td>
        <td>
            1.分布式存储 <br>
            2.分布式事务 <br> 
            3.分布式锁-redis  <br>
            4.一致性算法  <br>
        </td>  
    </tr>
    <tr>
        <td>幂等</td>  
        <td>
            1.接口幂等 <br>
            2.MQ消息幂等-消息丢失/重复
        </td>  
    </tr>
    <tr>
        <td>数据一致性</td>  
        <td>
            1.数据库与缓存数据一致性
        </td>  
    </tr>
    <tr>
        <td>性能优化</td>
        <td>
            1.后端性能优化 
            2.前端性能优化
        </td>
    </tr>
    </th columnspan="3">
    <tr>
        <td rowspan="6">MiddleWare</td>
        <td>Elastic Stack</td>
        <td>
            1.架构 <br>
            2.核心原理分析
        </td>    
    </tr>
    <tr>
        <td>Kafka</td>
        <td>
            1.架构 <br>
            2.producer <br>
            3.Consumer <br>
            4.问题-解决记录 <br>
            5.面试题 <br>
            6.顺序消费问题 <br>
            7.时间轮算法 <br>
            8.消息补偿机制 <br>
            9.testing <br>
        </td>    
    </tr>
    <tr>
        <td>Redis</td>
        <td>
            1.基础架构 <br>
            2.数据结构 <br>
            3.布隆过滤器 <br>
            4.缓存雪崩-缓存穿透 <br>
            5.过期策略和内存淘汰机制 <br>
            6.持久化 <br>
        </td>
    </tr>
    <tr>
        <td>API GateWay</td>
        <td>
            1.基本功能与定位
        </td>
    </tr>
    <tr>
        <td>Apache Camel</td>
        <td>
            1.基本功能与定位
        </td>
    </tr>  
    <tr>
        <td>Apache Cassandra</td>
        <td>
            1.core <br>
            2.内部机制 <br>
            3.clusters <br>
        </td>
    </tr>    
    </th columnspan="3">
    <tr>
        <td rowspan="5">Java</td>
        <td>核心</td>
        <td>
            1.对象创建及分配<br />
            2.类加载<br />
            3.GC<br />
            4.JVM运行时数据区<br /> 
        </td>
    </tr>
    <tr>
        <td>JUC</td>
        <td>
            1.锁<br />
            2.AQS<br />
            3.ThreadLocal<br />
            4.CompletableFuture<br />
            5.ThreadPool
        </td>
    </tr>
    <tr>
        <td>Guava</td>
        <td>
            1.RateLimiter<br />
        </td>
    </tr>
    <tr>
        <td>Netty</td>
        <td>
            1.core <br />
            2.EventLoop <br />
            3.Clusters <br />
            4.Work-flow <br />
            5.参数 <br />
            6.FastThreadLocal <br />
        </td>
    </tr>
</table>


<br>
<br>
<br>
---

| Name               | Content                                                      |
| ------------------ | ------------------------------------------------------------ |
| Architecture       | DDD| 
| Cloud Compute      | 1.GCP<br />2.Cloud Foundry                                   |
| JavaScript         |                                                              |
| Kubernetes         |                                                              |
| Node               |                                                              |
| RDBMS              | 1.Mysql                                                      |
| Sap                | 1.Sap Hana                                                   |
|                    |                                                              |
| 算法与数据结构       |                                                              |
|                    |                                                              |

<br>
---
<br>

# Example:

## Java

| # | content | drawio |
| --- | ------- | ------ |
| 1 | 对象创建及分配 | ![对象创建与分配](images/对象创建及分配.png) |
| 2 | 类加载 | ![classload](images/类加载.png) |
| 3 | 锁 | ![lock](images/锁.png) |
| 4 | AQS | ![aqs](images/AQS-及其关联的同步工具类-关系流程图.svg) |
| 5 | GC | ![gc](images/GC.png) |
| 6 | JVM | ![jvm](images/JVM.png) |
| 7 | ThreadPool | ![ThreadPool](images/ThreadPool.svg) |
| 8 | Guava-Ratelimiter | ![ratelimiter](images/ratelimiter.svg) |
| 9 | | |
| 10 | | |








---




