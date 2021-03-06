### 缓存穿透、缓存击穿、缓存雪崩的介绍以及应对方案

> 缓存穿透  

问题描述：一个查询请求过来，缓存层没有缓存，数据库也没有对应的数据。

应对方案：  
1. 做好请求参数的校验（治标不治本）  
2. 缓存null值  
3. 使用布隆过滤器  **推荐**

> 缓存击穿  

问题描述： 一个查询请求过来，缓存层没有，数据库有，就好像在缓存层打了一个洞，如果并发量大的话，就会造成服务端的巨大压力甚至崩溃。

应对方案：

1. 监控热点流量，及时反映，或者热点key干脆不设置过期时间（比如季节性的业务可以提前增加服务器数量）
2. 通过限流工具（Hystrix、sentinel、Guava RateLimiter），保证服务可用 **推荐**
3. 加锁 如果你们使用的是SpringCache `@Cacheable(value = xxx, key = "#id", sync = true)`, 也可以根据这个思路实现自己的加锁

> 缓存雪崩  

问题描述：同一时间有大量的key失效，请求集中打到数据库层。

应对方案：

1. 监控热点流量  
2. 不同缓存失效时间不能一致  
3. 同一类型的缓存时间也尽量随机  

> 缓存一致性  

**基调： 没有完美的一致性方案，只有符合业务的方案。**  
这个问题很宽泛，可以有很多中实现。

1. 双删  
适合并发量不高的接口 因为删除缓存的时候如果大量请求过来之后会导致服务器服务不可用。第二次删除缓存的意思是为了防止在第一次删除之后其他线程又查询数据库将旧数据设置到缓存造成数据不一致。  
弱一致性

```java

public void updateUserInfo(User user) {                                       
    // 删除缓存
    redisTemplate.delete(user.getId());
    // 更新数据库
    userDao.update(user);
    // 删除缓存 这样的实现存在稍微的问题，如果第二次删除失败，并且其他线程也更新了缓存，就会造成缓存不一致
    redisTemplate.delete(user.getId());
  }
```
2. 多级缓存  


3. 加锁  



4. 异步更新  
同过mq、观察者、发布订阅等手段更新缓存， 时效性差。准一致性  我们的业务是通过canal订阅mysql的binlog再将消息推到kafka，再消费更新缓存。

5. 定时任务  
通过任务调度框架，按照一定频率更新缓存，最终一致性。