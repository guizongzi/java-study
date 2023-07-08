在Spring Boot中，可以使用`@CachePut`、`@Cacheable`和`@CacheEvict`这些注解来实现缓存功能。这些注解是Spring框架提供的缓存注解，用于简化缓存操作的配置和使用。

这些缓存注解可以应用在Spring Boot的Service层或DAO层的方法上，用于提高方法的执行效率和性能。通过配置合适的缓存管理器和缓存策略，可以实现自动缓存数据，并在需要时从缓存中获取数据，从而减少对数据库或其他耗时操作的访问。

在对相关方法进行注解前，应在启动程序上加上`@EnableCaching`注解，以支持缓存模式

### `@Cacheable`注解：

- **作用：**从缓存中获取值，如果缓存中存在该值，则直接返回，否则执行方法体，并将返回值放入缓存中。

- **用法：**在需要使用缓存的方法上添加`@Cacheable`注解，并指定缓存的名称和缓存的键。

- **示例：**

  ```java
  @Cacheable(value = "myCache", key = "#id")
  public User getUserById(Long id) {
  }
  ```

### `@CachePut`注解：

- **作用：**将方法的返回值放入缓存中，每次方法调用都会执行方法体，并将返回值缓存起来。
- **用法：**在需要缓存的方法上添加`@CachePut`注解，并指定缓存的名称和缓存的键。
- **示例：**
  
  ```java
  @CachePut(value = "myCache", key = "#id")
  public User getUserById(Long id) {
  }
  ```

### `@CacheEvict`注解：

- **作用：**从缓存中移除值。
- **用法：**在需要移除缓存值的方法上添加`@CacheEvict`注解，并指定缓存的名称和缓存的键。
- **示例：**
  
  ```java
  @CacheEvict(value = "myCache", key = "#id")
  public void deleteUserById(Long id) {
  }
  ```

