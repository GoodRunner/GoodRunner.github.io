## synchronized之类锁和对象锁

在多线程中我们需要对`共享资源`加锁来避免线程安全问题,其中`synchronized`是java中比较方便的锁实现，那么使用这个关键字我们大致有以下几种场景：

1. `synchronized`修饰非静态方法

   ```java
   synchronized public void getValue() {}
   ```

   

2. `synchronized`修饰静态方法

   ```java
   synchronized static public void getValue() {}
   ```

   

>  talk is cheap show me the code 