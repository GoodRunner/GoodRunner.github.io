### java Exception 架构

架构图  

> Throwable为基类，Error和Exception继承Throwable。

其中，`Error`和`RuntimeException`及其子类成为`未检查异常`（unchecked），其它异常成为`已检查异常`（checked）。   

![exception](/img/java_exception.jpeg)  

> Spring事物以及配置  

Spring的事物管理默认是针对unchecked exception回滚，也就是默认对Error异常和RuntimeException异常以及其子类进行事物回滚，且必须对抛出异常，若使用try-catch对其异常捕获则不会进行回滚！（Error异常和RuntimeException异常抛出时不需要方法调用throws或try-catch语句）； checked异常，checked异常必须由try-catch语句包含或者由方法throws抛出，且事物默认对checked异常不进行回滚。所以如果需要对checked异常进行回滚得在catch中手动抛出自定义的`RuntimeException`  

```
//自定义异常
public class ServiceException extends RuntimeException {

  private Integer code;
  private String errorMessage;

  /**
   * 服务异常构建函数.
   */
  public ServiceException(Integer code, String errorMessage) {
    super(errorMessage);
    this.code = code;
    this.errorMessage = errorMessage;
  }

  public ServiceException(MessageEnum exception) {
    this.code = exception.getCode();
    this.errorMessage = exception.getMsg();
  }
}
```
这样就能回滚了
```$xslt
        try {
          // 有可能抛出checked exception 的地方
        } catch (Exception e) {
          // 抛出自定义的runtime exception
          throw new  ServiceException(ResponseCodeEnum.JSON_PARSE_ERROR.getCode(), ResponseCodeEnum.JSON_PARSE_ERROR.getMsg());
        }
```

或者 在Transaction中定义noRollbackFor和RollBackFor指定某种异常是否回滚。
```$xslt
@Transaction(noRollbackFor=RuntimeException.class) 
@Transaction(RollbackFor=Exception.class) 
```
 https://blog.csdn.net/qq_34581118/article/details/81389218