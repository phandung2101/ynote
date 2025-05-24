Using `restTemplate` could be very annoy overtime. So Spring come up with more convinience declarative.
> Read detail on: https://spring.io/projects/spring-cloud-openfeign#overview.

# Quick example
```java
@FeignClient(name = "token-service", url = "http://localhost:8080/internal/token")  
public interface TokenVerifyHttpClient {  
  
    @PostMapping("/validate")  
    TokenValidationResponse validateToken(@RequestBody TokenValidationRequest request);  
  
}
```
Very simple use.
# Some helpful config

## Custom Error Handling
By default, Feign throws a `FeignException` for non-2xx responses. You can define a custom error decoder:
```java
import feign.Response;
import feign.codec.ErrorDecoder;

public class CustomErrorDecoder implements ErrorDecoder {
    @Override
    public Exception decode(String methodKey, Response response) {
        if (response.status() == 404) {
            return new RuntimeException("User not found");
        }
        return new FeignException(response.status(), "An error occurred");
    }
}
```
## Timeout
```
feign.client.config.default.connectTimeout=5000
feign.client.config.default.readTimeout=5000
```
## Customize http client library
```java
@Configuration
public class FeignConfig {
    @Bean
    public feign.okhttp.OkHttpClient feignOkHttpClient() {
        return new feign.okhttp.OkHttpClient();
    }
}
```
## Using with Eureka (Service Discovery)
```java
@FeignClient(name = "user-service") // don't need to use url
public interface UserClient {
    @GetMapping("/users/{id}")
    User getUserById(@PathVariable("id") Long id);
}
```
Add dependency
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```
## Circuit Breaker
```java
@FeignClient(name = "user-service", fallback = UserClientFallback.class)
public interface UserClient {
    @GetMapping("/users/{id}")
    User getUserById(@PathVariable("id") Long id);
}

@Component
public class UserClientFallback implements UserClient {
    @Override
    public User getUserById(Long id) {
        return new User(id, "Fallback User", "fallback@example.com");
    }
}
```