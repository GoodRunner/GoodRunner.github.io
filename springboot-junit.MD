## springboot下的单元测试

该文章在springboot2.0+环境下运行

引入包

```
<dependency>
   			<groupId>org.springframework.boot</groupId>
   			<artifactId>spring-boot-starter-test</artifactId>
   			<scope>test</scope>
</dependency>
```

1. 普通常用的junit测试
直接点击运行方法就可启动该方法在springboot下的测试，可以注入相应的`service` ,进行测试
```
@RunWith(SpringRunner.class)
@SpringBootTest
public class StucentTest {

  @Test
  public void startTest() throws Exception {
    
    System.out.println("1234");
    
  }

}
```

2. 单个类中的测试方法顺序
只需要在类上面加上 `@FixMethodOrder(MethodSorters.NAME_ASCENDING)`注解，其中`MethodSorters`,有三个值`JVM`,`DEFAULT`,`NAME_ASCENDING`,jvm不保证顺序，default和name(我测试他们都按照方法名字典排序来执行)如果你需要一定顺序执行，就把方法名称写的按照字母顺序。

```
        /**
        * Sorts the test methods by the method name, in lexicographic order,
        * with {@link Method#toString()} used as a tiebreaker
        */
       NAME_ASCENDING(MethodSorter.NAME_ASCENDING),
   
       /**
        * Leaves the test methods in the order returned by the JVM.
        * Note that the order from the JVM may vary from run to run
        */
       JVM(null),
   
       /**
        * Sorts the test methods in a deterministic, but not predictable, order
        */
       DEFAULT(MethodSorter.DEFAULT);
```
示例

```

@RunWith(SpringRunner.class)
@SpringBootTest
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class ErrorBookServiceTests {

  @Autowired
  private ExamDao ExamDao;

  @Test
  public  void testA() {

    GreExam exam = new GreExam();

    exam.setUsed(1);

    List<GreExam>  list = ExamDao.queryAll(exam);

  }
}  
```

3. 类之间按照顺序执行，统一的一个测试类

这里需要用到`TestSuite`,把所有的测试类容纳到一个类中进行统一测试，就可以指定类之间的执行顺序,这个类中不需要写任何代码，只需要在`@Suite.SuiteClasses`注解中把你想测试的类按照一定顺序写出来即可。然后手动运行这个测试类。
```java
@RunWith(Suite.class)
@Suite.SuiteClasses({ExamTest.class, ErrorBookServiceTests.class})
@SpringBootTest
public class AllTest {

}
```

4. 统一测试和`gradle`集成

在`gradle`配置文件中添加上`test`指向这个总类就可以，再运行`./gradlew check`就会运行总的test

```
test {
    include '**/AllGreTest*'
}
```

5. 统一测试和`maven`的集成

需要添加`maven-surefire-plugin`,如果你执行过`mvn test`或者执行其他`maven`命令时跑了测试用例，你就已经用过`maven-surefire-plugin`了。`maven-surefire-plugin`是`maven`里执行测试用例的插件，不显示配置就会用默认配置。如果需要其他配置就需要显示配置。把`include`指向你的总的测试文件就行。

```text
 <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.19.1</version>
                <configuration>
                    <includes>
                        <include>**/AllTests.java</include>
                    </includes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

