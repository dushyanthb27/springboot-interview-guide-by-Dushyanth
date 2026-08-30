# Spring Boot Annotations Cheatsheet

Quick reference guide for commonly used Spring Boot annotations.

## Core Spring Annotations

### @SpringBootApplication
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
Combines: @Configuration, @EnableAutoConfiguration, @ComponentScan

### @Configuration
```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```
Indicates a class declares bean definitions.

### @Component
```java
@Component
public class MyComponent {
    // Component implementation
}
```
Generic stereotype for any Spring-managed component.

### @Service
```java
@Service
public class MyService {
    // Service layer implementation
}
```
Indicates a service layer component.

### @Repository
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Repository methods
}
```
Indicates a data access component, enables exception translation.

### @Controller
```java
@Controller
public class MyController {
    // Controller implementation
}
```
Indicates a Spring MVC controller.

### @RestController
```java
@RestController
@RequestMapping("/api")
public class MyRestController {
    // REST controller implementation
}
```
Combines @Controller and @ResponseBody for REST APIs.

## Dependency Injection

### @Autowired
```java
@Autowired
private MyService myService;

// Constructor injection (recommended)
@Autowired
public MyController(MyService myService) {
    this.myService = myService;
}
```
Automatic dependency injection.

### @Qualifier
```java
@Autowired
@Qualifier("specificService")
private MyService myService;
```
Disambiguates beans when multiple beans of the same type exist.

### @Primary
```java
@Primary
@Bean
public MyService primaryService() {
    return new MyService();
}
```
Indicates a bean should be preferred when multiple beans exist.

### @Value
```java
@Value("${app.name}")
private String appName;

@Value("${app.timeout:5000}")
private int timeout;
```
Injects property values from configuration files.

## Web MVC Annotations

### @RequestMapping
```java
@RequestMapping("/users")
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public List<User> getUsers() {
        return userService.findAll();
    }
}
```
Maps HTTP requests to handler methods.

### @GetMapping
```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
```
Maps HTTP GET requests.

### @PostMapping
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.save(user);
}
```
Maps HTTP POST requests.

### @PutMapping
```java
@PutMapping("/users/{id}")
public User updateUser(@PathVariable Long id, @RequestBody User user) {
    return userService.update(id, user);
}
```
Maps HTTP PUT requests.

### @DeleteMapping
```java
@DeleteMapping("/users/{id}")
public void deleteUser(@PathVariable Long id) {
    userService.delete(id);
}
```
Maps HTTP DELETE requests.

### @PatchMapping
```java
@PatchMapping("/users/{id}")
public User patchUser(@PathVariable Long id, @RequestBody User user) {
    return userService.patch(id, user);
}
```
Maps HTTP PATCH requests.

### @PathVariable
```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
```
Binds URI template variables to method parameters.

### @RequestParam
```java
@GetMapping("/users")
public List<User> getUsers(@RequestParam(defaultValue = "0") int page,
                           @RequestParam(defaultValue = "10") int size) {
    return userService.findAll(page, size);
}
```
Binds request parameters to method parameters.

### @RequestBody
```java
@PostMapping("/users")
public User createUser(@RequestBody @Valid User user) {
    return userService.save(user);
}
```
Binds HTTP request body to a method parameter.

### @ResponseBody
```java
@GetMapping("/data")
@ResponseBody
public String getData() {
    return "response data";
}
```
Indicates return value should be written to HTTP response body.

### @ResponseStatus
```java
@ResponseStatus(HttpStatus.CREATED)
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.save(user);
}
```
Sets the HTTP response status code.

## Data Access Annotations

### @Entity
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```
Marks a class as a JPA entity.

### @Id
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```
Specifies the primary key.

### @GeneratedValue
```java
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```
Specifies the primary key generation strategy.

### @Column
```java
@Column(name = "user_name", nullable = false, length = 100)
private String name;
```
Customizes column mapping.

### @Table
```java
@Entity
@Table(name = "users", indexes = {
    @Index(name = "idx_user_name", columnList = "name")
})
public class User {
    // Entity fields
}
```
Specifies the table name and constraints.

### @OneToMany
```java
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
private List<Order> orders = new ArrayList<>();
```
Defines a one-to-many relationship.

### @ManyToOne
```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "user_id", nullable = false)
private User user;
```
Defines a many-to-one relationship.

### @ManyToMany
```java
@ManyToMany
@JoinTable(name = "user_roles",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "role_id"))
private Set<Role> roles = new HashSet<>();
```
Defines a many-to-many relationship.

### @OneToOne
```java
@OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
@JoinColumn(name = "profile_id")
private Profile profile;
```
Defines a one-to-one relationship.

### @Transactional
```java
@Transactional
public void createUserWithOrders(User user, List<Order> orders) {
    userService.save(user);
    orderService.saveAll(orders);
}
```
Defines transaction boundaries.

### @Query
```java
@Query("SELECT u FROM User u WHERE u.email = :email")
User findByEmail(@Param("email") String email);

@Query(value = "SELECT * FROM users WHERE status = :status", nativeQuery = true)
List<User> findByStatus(@Param("status") String status);
```
Defines custom JPQL or native SQL queries.

## Validation Annotations

### @Valid
```java
@PostMapping("/users")
public User createUser(@RequestBody @Valid User user) {
    return userService.save(user);
}
```
Triggers validation of the annotated object.

### @NotNull
```java
@NotNull(message = "Name cannot be null")
private String name;
```
Validates that a field is not null.

### @NotBlank
```java
@NotBlank(message = "Name cannot be blank")
private String name;
```
Validates that a string is not null or empty.

### @Size
```java
@Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
private String name;
```
Validates the size of collections, strings, or arrays.

### @Email
```java
@Email(message = "Invalid email format")
private String email;
```
Validates that a string is a valid email address.

### @Min / @Max
```java
@Min(value = 18, message = "Age must be at least 18")
@Max(value = 120, message = "Age must be at most 120")
private int age;
```
Validates numeric ranges.

### @Pattern
```java
@Pattern(regexp = "^[A-Za-z0-9]+$", message = "Only alphanumeric characters allowed")
private String username;
```
Validates against a regular expression.

## Spring Boot Specific

### @SpringBootApplication
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
Main application annotation.

### @EnableAutoConfiguration
```java
@Configuration
@EnableAutoConfiguration
public class MyConfiguration {
    // Configuration
}
```
Enables Spring Boot's auto-configuration.

### @ConfigurationProperties
```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private int timeout;
    // Getters and setters
}
```
Binds external configuration to a bean.

### @ConditionalOnProperty
```java
@Bean
@ConditionalOnProperty(name = "app.feature.enabled", havingValue = "true")
public FeatureService featureService() {
    return new FeatureService();
}
```
Conditional bean creation based on property.

### @ConditionalOnClass
```java
@Bean
@ConditionalOnClass(name = "com.example.SomeClass")
public SomeService someService() {
    return new SomeService();
}
```
Conditional bean creation based on class presence.

### @ConditionalOnMissingBean
```java
@Bean
@ConditionalOnMissingBean
public DefaultService defaultService() {
    return new DefaultService();
}
```
Conditional bean creation if no matching bean exists.

## Spring Cloud Annotations

### @EnableDiscoveryClient
```java
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```
Enables service discovery.

### @EnableCircuitBreaker
```java
@SpringBootApplication
@EnableCircuitBreaker
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}
```
Enables circuit breaker pattern.

### @FeignClient
```java
@FeignClient(name = "user-service")
public interface UserClient {
    @GetMapping("/users/{id}")
    User getUser(@PathVariable("id") Long id);
}
```
Creates a declarative HTTP client.

### @EnableConfigServer
```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```
Enables configuration server.

### @EnableZuulProxy
```java
@SpringBootApplication
@EnableZuulProxy
public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```
Enables Zuul API gateway (deprecated, use Spring Cloud Gateway).

## Spring Security Annotations

### @EnableWebSecurity
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    // Security configuration
}
```
Enables web security.

### @PreAuthorize
```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) {
    userRepository.deleteById(id);
}
```
Method-level security with pre-invocation authorization.

### @PostAuthorize
```java
@PostAuthorize("returnObject.owner == authentication.name")
public User getUser(Long id) {
    return userRepository.findById(id);
}
```
Method-level security with post-invocation authorization.

### @Secured
```java
@Secured("ROLE_ADMIN")
public void adminOperation() {
    // Admin-only operation
}
```
Simple method-level security.

### @RolesAllowed
```java
@RolesAllowed("ADMIN")
public void adminOperation() {
    // Admin-only operation
}
```
JSR-250 method-level security.

## Testing Annotations

### @SpringBootTest
```java
@SpringBootTest
@AutoConfigureMockMvc
public class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testGetUser() throws Exception {
        mockMvc.perform(get("/api/users/1"))
               .andExpect(status().isOk());
    }
}
```
Integration test with full application context.

### @WebMvcTest
```java
@WebMvcTest(UserController.class)
public class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    public void testGetUser() throws Exception {
        mockMvc.perform(get("/api/users/1"))
               .andExpect(status().isOk());
    }
}
```
Tests only the web layer.

### @DataJpaTest
```java
@DataJpaTest
public class UserRepositoryTest {
    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private UserRepository userRepository;

    @Test
    public void testFindByName() {
        User user = new User("John");
        entityManager.persist(user);
        entityManager.flush();

        User found = userRepository.findByName("John");
        assertThat(found.getName()).isEqualTo("John");
    }
}
```
Tests JPA repositories.

### @MockBean
```java
@SpringBootTest
public class UserServiceTest {
    @MockBean
    private UserRepository userRepository;

    @Test
    public void testGetUser() {
        when(userRepository.findById(1L)).thenReturn(Optional.of(new User()));
        // Test logic
    }
}
```
Creates a mock bean for testing.

### @TestConfiguration
```java
@TestConfiguration
public class TestConfig {
    @Bean
    public TestService testService() {
        return new TestService();
    }
}
```
Test-specific configuration.

## Scheduling Annotations

### @EnableScheduling
```java
@Configuration
@EnableScheduling
public class SchedulingConfig {
    // Scheduling configuration
}
```
Enables scheduled tasks.

### @Scheduled
```java
@Scheduled(fixedRate = 5000)
public void performTask() {
    // Task executed every 5 seconds
}

@Scheduled(cron = "0 0 * * * *")
public void performDailyTask() {
    // Task executed every day at midnight
}
```
Schedules a method for periodic execution.

### @Async
```java
@EnableAsync
@Configuration
public class AsyncConfig {
    // Async configuration
}

@Async
public CompletableFuture<String> asyncMethod() {
    // Async method implementation
}
```
Enables asynchronous method execution.

## Aspect-Oriented Programming

### @Aspect
```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        // Before advice
    }
}
```
Marks a class as an aspect.

### @Before
```java
@Before("execution(* com.example.service.*.*(..))")
public void logBefore(JoinPoint joinPoint) {
    // Before advice
}
```
Before advice.

### @After
```java
@After("execution(* com.example.service.*.*(..))")
public void logAfter(JoinPoint joinPoint) {
    // After advice
}
```
After advice.

### @Around
```java
@Around("execution(* com.example.service.*.*(..))")
public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
    // Around advice
    return joinPoint.proceed();
}
```
Around advice.

### @AfterReturning
```java
@AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
public void logAfterReturning(JoinPoint joinPoint, Object result) {
    // After returning advice
}
```
After returning advice.

### @AfterThrowing
```java
@AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "error")
public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
    // After throwing advice
}
```
After throwing advice.

---

This cheatsheet provides a quick reference for the most commonly used Spring Boot annotations. For detailed usage, refer to the official Spring documentation.
