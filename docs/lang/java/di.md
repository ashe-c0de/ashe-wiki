> 在软件工程中，依赖注入（dependency injection，缩写为 DI）是一种软件设计模式，也是实现控制反转(IoC)的其中一种技术。这种模式能让一个物件接收它所依赖的其他物件。

举个例子，比如在`CarService`这个class中，需要使用到`DriverService`提供的某些功能，我们不妨称`DriverService`为`CarService`所需要的依赖，并将其注入到`CarService`中，使class拥有了新的成员变量`DriverService`实例，由此class便可使用`DriverService`的功能。

映射到现实生活，就是有一个提供WIFI的路由器，大家的手机在没有流量的前提下，都需要连接WIFI才能接入Internet，但不必每部手机都配备一台提供WIFI的路由器。

再回到代码世界，接口层Controller、业务层Service、数据库层Dao/Repository这种包结构的规范，其实就对应着依赖注入的具体表现，我们在接口层注入Service，在业务层注入Repository……

在一些不遵守规范的project中，如果业务层和数据库层划分不明确，发生了耦合，这就非常容易引发循环依赖的问题。

比如使用Mybatis-Plus的某些project中，可以看到类似这样的一个`Service`

```java
@Service
public class AppleServiceImpl extends ServiceImpl<AppleMapper, Apple> implements AppleService {
    
    // 这是一个错误的示例

}
```

这个`@Service`会令人误以为这个是一个业务类，但它`extends ServiceImpl<AppleMapper, Apple>`实际上又是数据库层的写法，于是就发生了业务层和数据库的耦合！因此它既是业务层又是数据库层。

之后，这个类中就会产生业务代码+数据库操作，当一个项目中这样的类越来越多，不可避免的会发生循环依赖，因为其他业务层（比如 ManServiceImpl）有可能在某一天需要操作Apple实体对应的数据库，于是依赖注入`AppleServiceImpl`使用Mybatis-Plus提供的常用方法。

过来一段时间，新的需求让另一个程序员，需要在`AppleServiceImpl`中操作Man实体对应的数据库，于是ta顺其自然地模仿project中其它代码的写法，在这个类中注入了`ManServiceImpl`，循环依赖就发生了，就需要考虑使用@Lazy注解了。

综上来看，大多时候循环依赖的根源就在于业务层与数据库层的耦合，如果一开始这样写

```java
@Repository
public class AppleRepository extends ServiceImpl<AppleMapper, Apple> {
}
```
```java
@Service
public class AppleService {

    @Resource
    private AppleRepository appleRepository;

}
```

那么业务层与数据库层就解耦了，在任何Service操作任何Repository的行为，已经从逻辑层面避免了循环依赖的可能性。

除此之外，依赖注入的方式在不同的project中也是千奇百怪，常见的有构造器注入、`@Autowired`注入、`@Resource`注入、`@RequiredArgsConstructor`注入。

当一个类中需要注入很多依赖时，建议使用`@RequiredArgsConstructor`来简化代码（使用`@Resource`结合`@Lazy`、`@Qualifier`应对特殊情况）。

> 构造器注入是 Spring 中最推荐的依赖注入方式，通过构造器注入，可以强制要求在创建类的实例时提供所有必需的依赖项，从而确保类的实例始终处于一个有效状态。这也使得依赖项的传递变得明确和类型安全。如果可能的话，尽量使用构造器注入。
