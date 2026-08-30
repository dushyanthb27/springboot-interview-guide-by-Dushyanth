# Spring Boot Configuration Patterns Cheatsheet

Quick reference for common Spring Boot configuration patterns and best practices.

## Application Properties

### Basic Application Properties
```properties
# Server Configuration
server.port=8080
server.servlet.context-path=/api

# Application Name
spring.application.name=user-service

# Logging
logging.level.root=INFO
logging.level.com.example=DEBUG

# Actuator
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.health.show-details=always
```

### Database Configuration
```properties
# H2 Database (Development)
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true

# PostgreSQL (Production)
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=postgres
spring.datasource.password=password
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA/Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

### MongoDB Configuration
```properties
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.database=mydb
spring.data.mongodb.username=user
spring.data.mongodb.password=password
```

### Redis Configuration
```properties
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=
spring.redis.database=0
spring.redis.timeout=60000
```

### RabbitMQ Configuration
```properties
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
spring.rabbitmq.listener.simple.concurrency=3
spring.rabbitmq.listener.simple.max-concurrency=10
```

### Kafka Configuration
```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

## Java-Based Configuration

### Basic Configuration Class
```java
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }

    @Bean
    public MyRepository myRepository() {
        return new MyRepository();
    }
}
```

### Configuration with Properties
```java
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private int timeout;
    private Database database;

    // Getters and setters

    public static class Database {
        private String url;
        private String username;
        private String password;

        // Getters and setters
    }
}
```

### REST Client Configuration
```java
@Configuration
public class WebClientConfig {

    @Bean
    public WebClient webClient() {
        return WebClient.builder()
                .baseUrl("https://api.example.com")
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .build();
    }

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

### Security Configuration
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults());
        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withUsername("user")
                .password(passwordEncoder().encode("password"))
                .roles("USER")
                .build();
        return new InMemoryUserDetailsManager(user);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### Database Configuration
```java
@Configuration
@EnableJpaRepositories(basePackages = "com.example.repository")
public class DatabaseConfig {

    @Bean
    public DataSource dataSource() {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
        config.setUsername("postgres");
        config.setPassword("password");
        config.setMaximumPoolSize(10);
        return new HikariDataSource(config);
    }

    @Bean
    public LocalContainerEntityManagerFactoryBean entityManagerFactory() {
        LocalContainerEntityManagerFactoryBean em = new LocalContainerEntityManagerFactoryBean();
        em.setDataSource(dataSource());
        em.setPackagesToScan("com.example.entity");
        em.setJpaVendorAdapter(new HibernateJpaVendorAdapter());
        return em;
    }

    @Bean
    public PlatformTransactionManager transactionManager() {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(entityManagerFactory().getObject());
        return transactionManager;
    }
}
```

### Cache Configuration
```java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public CacheManager cacheManager() {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(10))
                .disableCachingNullValues();

        return RedisCacheManager.builder()
                .cacheDefaults(config)
                .build();
    }
}
```

### Async Configuration
```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("Async-");
        executor.initialize();
        return executor;
    }
}
```

### Scheduling Configuration
```java
@Configuration
@EnableScheduling
public class SchedulingConfig {
    // Scheduling configuration
}
```

## Profile-Specific Configuration

### Profile-Based Properties
```properties
# application-dev.properties
spring.datasource.url=jdbc:h2:mem:devdb
logging.level.root=DEBUG

# application-prod.properties
spring.datasource.url=jdbc:postgresql://prod-server:5432/mydb
logging.level.root=WARN
```

### Profile-Based Configuration
```java
@Configuration
@Profile("dev")
public class DevConfig {

    @Bean
    public DevService devService() {
        return new DevService();
    }
}

@Configuration
@Profile("prod")
public class ProdConfig {

    @Bean
    public ProdService prodService() {
        return new ProdService();
    }
}
```

### Conditional Configuration
```java
@Configuration
public class ConditionalConfig {

    @Bean
    @ConditionalOnProperty(name = "app.feature.enabled", havingValue = "true")
    public FeatureService featureService() {
        return new FeatureService();
    }

    @Bean
    @ConditionalOnMissingBean(DataSource.class)
    public DataSource defaultDataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
    }
}
```

## Spring Cloud Configuration

### Eureka Server Configuration
```properties
# Eureka Server
server.port=8761
eureka.instance.hostname=localhost
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

### Eureka Client Configuration
```properties
# Eureka Client
spring.application.name=user-service
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.instance.prefer-ip-address=true
```

### Config Server Configuration
```properties
# Config Server
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/username/config-repo
spring.cloud.config.server.git.clone-on-start=true
```

### Config Client Configuration
```properties
# Config Client
spring.cloud.config.uri=http://localhost:8888
spring.cloud.config.name=user-service
spring.cloud.config.profile=dev
```

### Gateway Configuration
```properties
# Gateway
server.port=8080
spring.application.name=api-gateway
spring.cloud.gateway.discovery.locator.enabled=true
spring.cloud.gateway.routes[0].id=user-service
spring.cloud.gateway.routes[0].uri=lb://user-service
spring.cloud.gateway.routes[0].predicates[0]=Path=/api/users/**
```

### Resilience4j Configuration
```properties
# Circuit Breaker
resilience4j.circuitbreaker.instances.user-service.register-health-indicator=true
resilience4j.circuitbreaker.instances.user-service.sliding-window-size=10
resilience4j.circuitbreaker.instances.user-service.failure-rate-threshold=50
resilience4j.circuitbreaker.instances.user-service.wait-duration-in-open-state=10000

# Retry
resilience4j.retry.instances.user-service.max-attempts=3
resilience4j.retry.instances.user-service.wait-duration=1000
```

## Testing Configuration

### Test Configuration
```java
@TestConfiguration
public class TestConfig {

    @Bean
    @Primary
    public UserService testUserService() {
        return new TestUserService();
    }
}
```

### Test Properties
```properties
# application-test.properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.jpa.hibernate.ddl-auto=create-drop
logging.level.root=WARN
```

## Actuator Configuration

### Actuator Properties
```properties
# Actuator
management.endpoints.web.exposure.include=health,info,metrics,prometheus
management.endpoint.health.show-details=always
management.metrics.export.prometheus.enabled=true
management.health.diskspace.enabled=true
management.health.redis.enabled=true
```

### Custom Health Indicator
```java
@Component
public class CustomHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        // Custom health check logic
        return Health.up().withDetail("custom", "OK").build();
    }
}
```

### Custom Metrics
```java
@Component
public class CustomMetrics {

    private final MeterRegistry meterRegistry;

    public CustomMetrics(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
    }

    public void recordCustomMetric(String value) {
        meterRegistry.counter("custom.metric", "value", value).increment();
    }
}
```

## Common Patterns

### Factory Pattern
```java
@Configuration
public class FactoryConfig {

    @Bean
    public ServiceFactory serviceFactory() {
        return new ServiceFactory();
    }

    @Bean
    public MyService myService(ServiceFactory factory) {
        return factory.createService("my-service");
    }
}
```

### Strategy Pattern
```java
@Configuration
public class StrategyConfig {

    @Bean
    public StrategyRegistry strategyRegistry(
            List<Strategy> strategies) {
        StrategyRegistry registry = new StrategyRegistry();
        strategies.forEach(registry::register);
        return registry;
    }
}
```

### Builder Pattern
```java
@Configuration
public class BuilderConfig {

    @Bean
    public MyObject myObject() {
        return MyObject.builder()
                .name("example")
                .value(100)
                .build();
    }
}
```

## Best Practices

### Configuration Organization
1. **Separate concerns**: Use different configuration classes for different concerns
2. **Use profiles**: Separate development and production configurations
3. **Externalize configuration**: Use properties files or config servers
4. **Type-safe configuration**: Use @ConfigurationProperties
5. **Conditional beans**: Use @Conditional annotations for flexibility

### Security Best Practices
1. **Never hardcode secrets**: Use environment variables or secret management
2. **Use HTTPS**: Configure SSL/TLS for production
3. **Enable CSRF**: For web applications, enable CSRF protection
4. **Use strong passwords**: For database and service authentication
5. **Regular updates**: Keep dependencies updated for security patches

### Performance Best Practices
1. **Connection pooling**: Use HikariCP for database connections
2. **Caching**: Implement caching for frequently accessed data
3. **Async processing**: Use @Async for long-running tasks
4. **Lazy loading**: Use FetchType.LAZY for JPA relationships
5. **Monitoring**: Use Actuator for performance monitoring

---

This cheatsheet provides common configuration patterns for Spring Boot applications. Adapt these patterns to your specific requirements and follow best practices for production deployments.
