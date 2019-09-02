# GoodRunner.github.io
myblog is here

> Filter和Interceptor 的 区别

这两者在功能方面很类似，但是在具体技术实现方面，差距还是比较大的，在分析两者的区别之前，我们先理解一下AOP的概念，AOP不是一种具体的技术，而是一种编程思想。在面向对象编程的过程中，我们很容易通过继承、多态来解决纵向扩展。 但是对于横向的功能，比如，在所有的service方法中开启事务，或者统一记录日志等功能，面向对象的是无法解决的。所以AOP——面向切面编程其实是面向对象编程思想的一个补充。而我们今天讲的过滤器和拦截器都属于面向切面编程的具体实现。

区别在于

1. Filter是依赖于Servlet容器，属于Servlet规范的一部分，而拦截器则是独立存在的，可以在任何情况下使用。

2. Filter的执行由Servlet容器回调完成，而拦截器通常通过动态代理的方式来执行。

3. Filter的生命周期由Servlet容器管理，而拦截器则可以通过IoC容器来管理，因此可以通过注入等方式来获取其他Bean的实例，因此使用会更方便。

> 过滤器

在springboot项目中配置过滤器大概有两种方式，普通方式和注解方式。这两种方式的执行顺序是有差别的，普通方式order是1的执行顺序是 过滤器-拦截器-controller，注解方式顺序就不一定了。
普通方式
```$xslt
public class LogCostFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
 
    }
 
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        long start = System.currentTimeMillis();
        filterChain.doFilter(servletRequest,servletResponse);
        System.out.println("Execute cost="+(System.currentTimeMillis()-start));
    }
 
    @Override
    public void destroy() {
 
    }
}
```

```$xslt
@Configuration
public class FilterConfig {
 
    @Bean
    public FilterRegistrationBean registFilter() {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(new LogCostFilter());
        registration.addUrlPatterns("/*");
        registration.setName("LogCostFilter");
        registration.setOrder(1);
        return registration;
    }
 
}
```

注解方式

```$xslt
@WebFilter(urlPatterns = "/*", filterName = "logFilter2")
public class LogCostFilter2 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
 
    }
 
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        long start = System.currentTimeMillis();
        filterChain.doFilter(servletRequest, servletResponse);
        System.out.println("LogFilter2 Execute cost=" + (System.currentTimeMillis() - start));
    }
 
    @Override
    public void destroy() {
 
    }
}
```