# Common Mistakes in Spring Boot Microservices

Frequent mistakes developers make when working with Spring Boot microservices and how to avoid them.

## Architecture Mistakes

### 1. Monolithic Microservices

**Mistake**: Creating microservices that are too small or not properly bounded, leading to distributed monoliths.

**Signs**:
- Services constantly need to call each other for simple operations
- Shared databases between services
- Tight coupling through shared libraries
- Deployment requires coordinating multiple services

**Solution**:
- Use Domain-Driven Design to define bounded contexts
- Ensure services are loosely coupled
- Implement proper API contracts
- Consider service size based on business capabilities

### 2. Ignoring Data Consistency

**Mistake**: Not planning for data consistency across services, leading to data integrity issues.

**Signs**:
- Data inconsistencies between services
- Lost updates or duplicate records
- Complex rollback scenarios

**Solution**:
- Implement Saga pattern for distributed transactions
- Use eventual consistency where appropriate
- Design compensating transactions
- Implement proper error handling and retry logic

### 3. Over-Engineering

**Mistake**: Adding unnecessary complexity by using patterns and technologies that aren't needed.

**Signs**:
- Using Kafka for simple service communication
- Implementing complex patterns for simple use cases
- Too many abstraction layers
- Unnecessary microservices decomposition

**Solution**:
- Start simple, add complexity when needed
- Evaluate trade-offs for each pattern
- Consider team size and expertise
- Focus on solving actual business problems

## Spring Boot Specific Mistakes

### 4. Misusing @Autowired

**Mistake**: Using field injection instead of constructor injection.

**Bad Practice**:
```java
@Component
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

**Good Practice**:
```java
@Component
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

**Why**: Constructor injection makes dependencies explicit, enables immutability, and facilitates testing.

### 5. Not Understanding Bean Scopes

**Mistake**: Using default singleton scope when prototype is needed, or vice versa.

**Common Issue**:
```java
@Bean
@Scope("prototype")
public RequestScopedBean requestScopedBean() {
    return new RequestScopedBean();
}

// Injecting into singleton bean causes issues
@Component
public class SingletonService {
    @Autowired
    private RequestScopedBean requestScopedBean; // Won't work as expected
}
```

**Solution**: Use ObjectProvider or ApplicationContext for prototype beans in singletons, or use proper scope proxies.

### 6. Ignoring Auto-Configuration

**Mistake**: Not understanding or misconfiguring auto-configuration, leading to unexpected behavior.

**Signs**:
- Beans not being created as expected
- Conflicting bean definitions
- Unwanted auto-configured beans

**Solution**:
- Use @Conditional annotations appropriately
- Exclude unwanted auto-configurations
- Use @AutoConfigurationPackage correctly
- Understand the auto-configuration report

## Database Mistakes

### 7. N+1 Query Problem

**Mistake**: Not optimizing JPA queries, leading to performance issues.

**Bad Practice**:
```java
@Entity
public class Order {
    @OneToMany(fetch = FetchType.LAZY)
    private List<OrderItem> items;
}

// This causes N+1 queries
List<Order> orders = orderRepository.findAll();
for (Order order : orders) {
    List<OrderItem> items = order.getItems(); // Triggers additional query
}
```

**Solution**:
```java
// Use JOIN FETCH
@Query("SELECT o FROM Order o JOIN FETCH o.items")
List<Order> findAllWithItems();

// Or use entity graphs
@EntityGraph(attributePaths = {"items"})
List<Order> findAll();
```

### 8. Not Using Connection Pooling

**Mistake**: Not configuring connection pooling, leading to performance issues.

**Solution**:
```properties
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.idle-timeout=600000
```

### 9. Ignoring Transaction Boundaries

**Mistake**: Not properly defining transaction boundaries, leading to data inconsistency.

**Bad Practice**:
```java
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;
    @Autowired
    private InventoryService inventoryService;

    public void createOrder(Order order) {
        orderRepository.save(order);
        inventoryService.reserveItems(order.getItems()); // Not in same transaction
    }
}
```

**Solution**:
```java
@Service
public class OrderService {
    @Transactional
    public void createOrder(Order order) {
        orderRepository.save(order);
        inventoryService.reserveItems(order.getItems());
    }
}
```

## Security Mistakes

### 10. Hardcoding Secrets

**Mistake**: Hardcoding passwords, API keys, or other sensitive information.

**Bad Practice**:
```java
@Value("${db.password}")
private String password = "hardcoded-password";
```

**Solution**:
```properties
# application.properties
db.password=${DB_PASSWORD}
```

```bash
# Environment variable
export DB_PASSWORD=secure-password
```

### 11. Not Validating Input

**Mistake**: Not validating user input, leading to security vulnerabilities.

**Solution**:
```java
@RestController
public class UserController {
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@Valid @RequestBody UserDTO userDTO) {
        // Validation happens automatically
        return ResponseEntity.ok(userService.createUser(userDTO));
    }
}

public class UserDTO {
    @NotBlank
    @Size(min = 2, max = 50)
    private String name;

    @Email
    private String email;
}
```

### 12. Misconfiguring CORS

**Mistake**: Allowing all origins in production, creating security risks.

**Bad Practice**:
```java
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**").allowedOrigins("*");
            }
        };
    }
}
```

**Solution**:
```java
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("https://trusted-domain.com")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("*")
                        .allowCredentials(true);
            }
        };
    }
}
```

## Performance Mistakes

### 13. Not Implementing Caching

**Mistake**: Not caching frequently accessed data, leading to poor performance.

**Solution**:
```java
@Service
public class UserService {
    @Cacheable(value = "users", key = "#id")
    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }

    @CacheEvict(value = "users", key = "#id")
    public void updateUser(Long id, User user) {
        userRepository.save(user);
    }
}
```

### 14. Synchronous Calls in High-Latency Scenarios

**Mistake**: Making synchronous calls to external services in high-latency scenarios.

**Bad Practice**:
```java
public OrderResponse processOrder(OrderRequest request) {
    User user = userService.getUser(request.getUserId()); // Blocking call
    Product product = productService.getProduct(request.getProductId()); // Blocking call
    // Process order
}
```

**Solution**:
```java
public Mono<OrderResponse> processOrder(OrderRequest request) {
    return Mono.zip(
            userService.getUser(request.getUserId()),
            productService.getProduct(request.getProductId())
    ).map(tuple -> {
        // Process order
        return new OrderResponse();
    });
}
```

### 15. Not Monitoring Performance

**Mistake**: Not implementing monitoring and metrics, making it hard to identify performance issues.

**Solution**:
```java
@Configuration
public class MetricsConfig {
    @Bean
    public MeterRegistryCustomizer<MeterRegistry> metricsCommonTags() {
        return registry -> registry.config().commonTags(
                "application", "user-service",
                "region", "us-east-1"
        );
    }
}

@Service
public class UserService {
    private final MeterRegistry meterRegistry;
    private final Counter userCreationCounter;

    public UserService(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.userCreationCounter = Counter.builder("user.creation")
                .description("Number of users created")
                .register(meterRegistry);
    }

    public User createUser(User user) {
        User savedUser = userRepository.save(user);
        userCreationCounter.increment();
        return savedUser;
    }
}
```

## Testing Mistakes

### 16. Not Testing Integration Points

**Mistake**: Only testing unit tests without integration tests.

**Solution**:
```java
@SpringBootTest
@AutoConfigureMockMvc
public class UserControllerIntegrationTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testCreateUser() throws Exception {
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"John\",\"email\":\"john@example.com\"}"))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.name").value("John"));
    }
}
```

### 17. Using @SpringBootTest for Unit Tests

**Mistake**: Using @SpringBootTest for simple unit tests, making them slow.

**Bad Practice**:
```java
@SpringBootTest
public class UserServiceTest {
    @Autowired
    private UserService userService;
    // This loads the entire application context
}
```

**Solution**:
```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;
    // This is a fast unit test
}
```

### 18. Not Using Testcontainers

**Mistake**: Using in-memory databases for tests instead of real databases.

**Solution**:
```java
@Testcontainers
@SpringBootTest
public class UserRepositoryTest {
    @Container
    private static final PostgreSQLContainer<?> postgres =
            new PostgreSQLContainer<>("postgres:14")
                    .withDatabaseName("testdb")
                    .withUsername("test")
                    .withPassword("test");

    @DynamicPropertySource
    static void postgresProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
}
```

## Deployment Mistakes

### 19. Not Health Checking

**Mistake**: Not implementing proper health checks, making it hard to monitor service health.

**Solution**:
```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    private final UserRepository userRepository;

    @Override
    public Health health() {
        try {
            userRepository.count();
            return Health.up().build();
        } catch (Exception e) {
            return Health.down(e).build();
        }
    }
}
```

### 20. Not Graceful Shutdown

**Mistake**: Not implementing graceful shutdown, leading to incomplete requests during deployment.

**Solution**:
```properties
server.shutdown=graceful
spring.lifecycle.timeout-per-shutdown-phase=30s
```

```java
@PreDestroy
public void cleanup() {
    // Cleanup resources
}
```

## General Best Practices

### Do's

- Use constructor injection
- Implement proper exception handling
- Add comprehensive logging
- Use appropriate caching strategies
- Implement circuit breakers for external calls
- Use proper testing strategies
- Monitor application performance
- Implement security best practices
- Use proper database connection pooling
- Configure appropriate bean scopes

### Don'ts

- Hardcode sensitive information
- Ignore data consistency in distributed systems
- Over-engineer simple problems
- Use field injection
- Ignore transaction boundaries
- Skip input validation
- Use synchronous calls in high-latency scenarios
- Skip integration testing
- Ignore monitoring and metrics
- Forget about graceful shutdown

## Learning from Mistakes

1. **Code Reviews**: Use code reviews to catch mistakes early
2. **Static Analysis**: Use tools like SonarQube to identify issues
3. **Monitoring**: Implement comprehensive monitoring to detect issues
4. **Testing**: Write comprehensive tests to prevent regressions
5. **Documentation**: Document lessons learned and best practices
6. **Continuous Learning**: Stay updated with best practices and patterns

---

Understanding these common mistakes and their solutions will help you write better, more robust Spring Boot microservices applications and avoid pitfalls in production environments.
