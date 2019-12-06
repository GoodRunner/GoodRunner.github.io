#### spring 事务传播机制  

spring中事务传播有以下七种类型

```text
  REQUIRED(0),
  SUPPORTS(1),
  MANDATORY(2),
  REQUIRES_NEW(3),
  NOT_SUPPORTED(4),
  NEVER(5),
  NESTED(6);
```

用法:  
![springTransaction](../img/springTransaction.png)

| 名称 | 含义 | 补充 |
|---|---|---|
| REQUIRED| 表示当前方法必须在事务中运行，如果当前有事务，则在该事务中运行，如果当前没有事务，则会启动一个新事务。 | |
| SUPPORTS| 表示当前方法不需要事务上下文，如果当前存在事务，则在方法该事务中运行，就是支持事务。 | |
| MANDATORY| 表示当前方法必须在事务中运行，如果当前事务不存在，则会抛出异常。 | |
| REQUIRES_NEW| 开启一个新的事务，如果当前存在事务，把当前事务挂起 | **如果有事务的两个方法在同一个类中，则不会开启新的事务** |
| NOT_SUPPORTED| 表示该方法不应该运行在事务中。如果存在当前事务，在该方法运行期间，当前事务将被挂起。 | |
| NEVER| 表示当前方法不应该运行在事务上下文中。如果当前正有一个事务在运行，则会抛出异常 | |
| NESTED| 表示如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作 | |