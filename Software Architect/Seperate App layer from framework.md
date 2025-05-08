Source: [https://www.arhohuttunen.com/hexagonal-architecture-spring-boot/](https://www.arhohuttunen.com/hexagonal-architecture-spring-boot/).

Using this component scan in outer layer to scan application layer.

```java
@Configuration
@ComponentScan(
        basePackages = "com.arhohuttunen.coffeeshop.application",
        includeFilters = @ComponentScan.Filter(
                type = FilterType.ANNOTATION, value = UseCase.class
        )
)
public class DomainConfig {
}

```

In application we can use custom layer to mark our service (use-case) like this

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Inherited
public @interface UseCase {
}

@UseCase
public class CoffeeShop implements OrderingCoffee {
    // ...
}
```'
```