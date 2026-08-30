# Microservices and Spring Boot Patterns Cheatsheet

Quick reference for common microservices and Spring Boot design patterns.

## Microservices Patterns

### Service Discovery Pattern
```java
// Eureka Server
@SpringBootApplication
@EnableEurekaServer
public class DiscoveryServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(DiscoveryServerApplication.class, args);
    }
}

// Eureka Client
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

// Service Discovery Usage
@Service
public class OrderService {
    private final DiscoveryClient discoveryClient;

    public OrderService(DiscoveryClient discoveryClient) {
        this.discoveryClient = discoveryClient;
    }

    public String getUserServiceUrl() {
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        return instances.get(0).getUri().toString();
    }
}
```

### API Gateway Pattern
```java
// Gateway Configuration
@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .uri("lb://user-service"))
                .route("order-service", r -> r
                        .path("/api/orders/**")
                        .uri("lb://order-service"))
                .build();
    }
}

// Gateway Filter
@Component
public class AuthFilter implements GlobalFilter {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        String token = exchange.getRequest().getHeaders().getFirst("Authorization");
        if (token == null || !token.startsWith("Bearer ")) {
            exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);
    }
}
```

### Circuit Breaker Pattern
```java
// Circuit Breaker Configuration
@Configuration
public class CircuitBreakerConfig {

    @Bean
    public CircuitBreaker circuitBreaker() {
        CircuitBreakerConfig config = CircuitBreakerConfig.custom()
                .failureRateThreshold(50)
                .waitDurationInOpenState(Duration.ofMillis(10000))
                .slidingWindowSize(10)
                .build();
        return CircuitBreaker.of("user-service", config);
    }
}

// Circuit Breaker Usage
@Service
public class UserServiceClient {

    private final CircuitBreaker circuitBreaker;
    private final RestTemplate restTemplate;

    public User getUser(Long id) {
        return circuitBreaker.executeSupplier(() ->
                restTemplate.getForObject("http://user-service/users/" + id, User.class)
        );
    }
}
```

### Retry Pattern
```java
// Retry Configuration
@Configuration
public class RetryConfig {

    @Bean
    public Retry retry() {
        return Retry.custom()
                .maxAttempts(3)
                .waitDuration(Duration.ofMillis(1000))
                .retryOnException(Exception.class)
                .build();
    }
}

// Retry Usage
@Service
public class ExternalServiceClient {

    private final Retry retry;
    private final RestTemplate restTemplate;

    public String callExternalService() {
        return Retry.decorateRunnable(retry, () ->
                restTemplate.getForObject("http://external-service/api", String.class)
        ).run();
    }
}
```

### Bulkhead Pattern
```java
// Bulkhead Configuration
@Configuration
public class BulkheadConfig {

    @Bean
    public Bulkhead bulkhead() {
        BulkheadConfig config = BulkheadConfig.custom()
                .maxConcurrentCalls(10)
                .maxWaitDuration(Duration.ofMillis(500))
                .build();
        return Bulkhead.of("service-bulkhead", config);
    }
}
```

### Rate Limiter Pattern
```java
// Rate Limiter Configuration
@Configuration
public class RateLimiterConfig {

    @Bean
    public RateLimiter rateLimiter() {
        return RateLimiter.custom()
                .limitForPeriod(100)
                .limitRefreshPeriod(Duration.ofSeconds(1))
                .timeoutDuration(Duration.ofMillis(500))
                .build();
    }
}
```

## Spring Boot Patterns

### Repository Pattern
```java
// Repository Interface
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
}

// Custom Repository Implementation
@Repository
public class UserRepositoryCustomImpl implements UserRepositoryCustom {

    @PersistenceContext
    private EntityManager entityManager;

    @Override
    public List<User> findUsersWithOrders() {
        String query = "SELECT u FROM User u JOIN FETCH u.orders";
        return entityManager.createQuery(query, User.class).getResultList();
    }
}
```

### Service Layer Pattern
```java
@Service
@Transactional
public class UserService {

    private final UserRepository userRepository;
    private final OrderService orderService;

    public UserService(UserRepository userRepository, OrderService orderService) {
        this.userRepository = userRepository;
        this.orderService = orderService;
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public User getUserWithOrders(Long id) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));
        user.setOrders(orderService.getOrdersByUserId(id));
        return user;
    }
}
```

### DTO Pattern
```java
// DTO Class
public class UserDTO {
    private Long id;
    private String name;
    private String email;

    // Constructors, getters, setters
}

// Mapper
@Component
public class UserMapper {

    public UserDTO toDTO(User user) {
        UserDTO dto = new UserDTO();
        dto.setId(user.getId());
        dto.setName(user.getName());
        dto.setEmail(user.getEmail());
        return dto;
    }

    public User toEntity(UserDTO dto) {
        User user = new User();
        user.setName(dto.getName());
        user.setEmail(dto.getEmail());
        return user;
    }
}
```

### Factory Pattern
```java
// Factory Interface
public interface NotificationFactory {
    Notification createNotification(String type);
}

// Concrete Factory
@Component
public class EmailNotificationFactory implements NotificationFactory {

    @Override
    public Notification createNotification(String type) {
        return new EmailNotification();
    }
}

// Factory Usage
@Service
public class NotificationService {

    private final Map<String, NotificationFactory> factories;

    public NotificationService(List<NotificationFactory> factoryList) {
        this.factories = factoryList.stream()
                .collect(Collectors.toMap(
                        factory -> factory.getClass().getSimpleName(),
                        Function.identity()
                ));
    }

    public void sendNotification(String type, String message) {
        NotificationFactory factory = factories.get(type + "Factory");
        Notification notification = factory.createNotification(type);
        notification.send(message);
    }
}
```

### Strategy Pattern
```java
// Strategy Interface
public interface PaymentStrategy {
    boolean processPayment(double amount);
}

// Concrete Strategies
@Component
public class CreditCardPayment implements PaymentStrategy {
    @Override
    public boolean processPayment(double amount) {
        // Credit card payment logic
        return true;
    }
}

@Component
public class PayPalPayment implements PaymentStrategy {
    @Override
    public boolean processPayment(double amount) {
        // PayPal payment logic
        return true;
    }
}

// Strategy Context
@Service
public class PaymentService {

    private final Map<String, PaymentStrategy> strategies;

    public PaymentService(List<PaymentStrategy> strategyList) {
        this.strategies = strategyList.stream()
                .collect(Collectors.toMap(
                        strategy -> strategy.getClass().getSimpleName(),
                        Function.identity()
                ));
    }

    public boolean processPayment(String method, double amount) {
        PaymentStrategy strategy = strategies.get(method);
        return strategy.processPayment(amount);
    }
}
```

### Template Method Pattern
```java
// Abstract Template
public abstract class DataProcessor {

    public final void processData() {
        loadData();
        validateData();
        transformData();
        saveData();
    }

    protected abstract void loadData();
    protected abstract void validateData();
    protected abstract void transformData();
    protected abstract void saveData();
}

// Concrete Implementation
@Service
public class UserDataProcessor extends DataProcessor {

    @Override
    protected void loadData() {
        // Load user data
    }

    @Override
    protected void validateData() {
        // Validate user data
    }

    @Override
    protected void transformData() {
        // Transform user data
    }

    @Override
    protected void saveData() {
        // Save user data
    }
}
```

### Observer Pattern
```java
// Event Publisher
@Component
public class UserEventPublisher {

    private final ApplicationEventPublisher eventPublisher;

    public void publishUserCreated(User user) {
        eventPublisher.publishEvent(new UserCreatedEvent(user));
    }
}

// Event Listener
@Component
public class UserEventListener {

    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        // Handle user created event
        sendWelcomeEmail(event.getUser());
    }

    @Async
    @EventListener
    public void handleUserCreatedAsync(UserCreatedEvent event) {
        // Async event handling
        updateUserStatistics(event.getUser());
    }
}
```

### Builder Pattern
```java
// Builder Class
public class UserBuilder {
    private Long id;
    private String name;
    private String email;

    public UserBuilder id(Long id) {
        this.id = id;
        return this;
    }

    public UserBuilder name(String name) {
        this.name = name;
        return this;
    }

    public UserBuilder email(String email) {
        this.email = email;
        return this;
    }

    public User build() {
        User user = new User();
        user.setId(id);
        user.setName(name);
        user.setEmail(email);
        return user;
    }
}

// Builder Usage
User user = new UserBuilder()
        .id(1L)
        .name("John Doe")
        .email("john@example.com")
        .build();
```

### Decorator Pattern
```java
// Base Component
public interface UserService {
    User getUser(Long id);
}

// Base Implementation
@Service
public class UserServiceImpl implements UserService {
    @Override
    public User getUser(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}

// Decorator
@Service
@Primary
public class CachedUserService implements UserService {

    private final UserService userService;
    private final CacheManager cacheManager;

    @Override
    public User getUser(Long id) {
        Cache cache = cacheManager.getCache("users");
        User user = cache.get(id, User.class);
        if (user == null) {
            user = userService.getUser(id);
            cache.put(id, user);
        }
        return user;
    }
}
```

## Data Patterns

### Repository Pattern with Specifications
```java
// Specification Interface
public interface UserSpecification {
    Predicate toPredicate(Root<User> root, CriteriaQuery<?> query, CriteriaBuilder cb);
}

// Specification Implementation
public class UserSpecificationImpl implements UserSpecification {

    @Override
    public Predicate toPredicate(Root<User> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
        List<Predicate> predicates = new ArrayList<>();

        if (name != null) {
            predicates.add(cb.like(root.get("name"), "%" + name + "%"));
        }

        if (email != null) {
            predicates.add(cb.equal(root.get("email"), email));
        }

        return cb.and(predicates.toArray(new Predicate[0]));
    }
}

// Repository with Specifications
@Repository
public interface UserRepository extends JpaRepository<User, Long>, JpaSpecificationExecutor<User> {
}

// Usage
Specification<User> spec = new UserSpecificationImpl("John", "john@example.com");
List<User> users = userRepository.findAll(spec);
```

### Unit of Work Pattern
```java
@Service
@Transactional
public class OrderService {

    private final OrderRepository orderRepository;
    private final ProductRepository productRepository;
    private final InventoryService inventoryService;

    public Order createOrder(Order order) {
        // All operations in a single transaction
        Product product = productRepository.findById(order.getProductId())
                .orElseThrow(() -> new ProductNotFoundException(order.getProductId()));

        inventoryService.decrementStock(product.getId(), order.getQuantity());

        Order savedOrder = orderRepository.save(order);

        return savedOrder;
    }
}
```

### CQRS Pattern
```java
// Command Side
@Service
public class UserCommandService {

    private final UserRepository userRepository;
    private final UserEventPublisher eventPublisher;

    public User createUser(CreateUserCommand command) {
        User user = new User(command.getName(), command.getEmail());
        User savedUser = userRepository.save(user);
        eventPublisher.publishUserCreated(savedUser);
        return savedUser;
    }
}

// Query Side
@Service
public class UserQueryService {

    private final UserReadModelRepository readModelRepository;

    public UserDTO getUser(Long id) {
        return readModelRepository.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));
    }

    public List<UserDTO> searchUsers(String query) {
        return readModelRepository.searchByName(query);
    }
}
```

## Testing Patterns

### Test Data Builder Pattern
```java
public class UserTestDataBuilder {
    private Long id = 1L;
    private String name = "Test User";
    private String email = "test@example.com";

    public static UserTestDataBuilder aUser() {
        return new UserTestDataBuilder();
    }

    public UserTestDataBuilder withId(Long id) {
        this.id = id;
        return this;
    }

    public UserTestDataBuilder withName(String name) {
        this.name = name;
        return this;
    }

    public UserTestDataBuilder withEmail(String email) {
        this.email = email;
        return this;
    }

    public User build() {
        User user = new User();
        user.setId(id);
        user.setName(name);
        user.setEmail(email);
        return user;
    }
}

// Usage in Tests
User user = UserTestDataBuilder.aUser()
        .withName("John Doe")
        .withEmail("john@example.com")
        .build();
```

### Object Mother Pattern
```java
public class UserObjectMother {

    public static User createDefaultUser() {
        User user = new User();
        user.setId(1L);
        user.setName("Default User");
        user.setEmail("default@example.com");
        return user;
    }

    public static User createAdminUser() {
        User user = createDefaultUser();
        user.setName("Admin User");
        user.setEmail("admin@example.com");
        user.setRole("ADMIN");
        return user;
    }
}

// Usage in Tests
User user = UserObjectMother.createDefaultUser();
```

## Integration Patterns

### Event-Driven Pattern
```java
// Event Publisher
@Service
public class OrderEventPublisher {

    private final RabbitTemplate rabbitTemplate;

    public void publishOrderCreated(Order order) {
        rabbitTemplate.convertAndSend("order.exchange", "order.created", order);
    }
}

// Event Consumer
@RabbitListener(queues = "order.created.queue")
public void handleOrderCreated(Order order) {
    // Process order created event
    inventoryService.reserveItems(order.getItems());
    notificationService.sendOrderConfirmation(order);
}
```

### Saga Pattern
```java
// Saga Orchestration
@Service
public class OrderSaga {

    private final OrderRepository orderRepository;
    private final PaymentService paymentService;
    private final InventoryService inventoryService;
    private final ShippingService shippingService;

    @SagaOrchestrationStart
    public void startOrderSaga(Order order) {
        try {
            // Step 1: Process payment
            paymentService.processPayment(order);

            // Step 2: Reserve inventory
            inventoryService.reserveItems(order.getItems());

            // Step 3: Create shipment
            shippingService.createShipment(order);

            // Complete saga
            order.setStatus(OrderStatus.COMPLETED);
            orderRepository.save(order);

        } catch (Exception e) {
            // Compensating transactions
            compensateOrder(order);
        }
    }

    private void compensateOrder(Order order) {
        paymentService.refundPayment(order);
        inventoryService.releaseItems(order.getItems());
        shippingService.cancelShipment(order);
        order.setStatus(OrderStatus.FAILED);
        orderRepository.save(order);
    }
}
```

## Security Patterns

### Authentication Filter Pattern
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtTokenProvider tokenProvider;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {
        String token = extractToken(request);

        if (token != null && tokenProvider.validateToken(token)) {
            Authentication auth = tokenProvider.getAuthentication(token);
            SecurityContextHolder.getContext().setAuthentication(auth);
        }

        filterChain.doFilter(request, response);
    }

    private String extractToken(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```

### Role-Based Access Control Pattern
```java
@PreAuthorize("hasRole('ADMIN')")
public void adminOperation() {
    // Admin-only operation
}

@PreAuthorize("hasAnyRole('ADMIN', 'MANAGER')")
public void managementOperation() {
    // Admin or manager operation
}

@PreAuthorize("#userId == authentication.principal.id or hasRole('ADMIN')")
public User getUser(Long userId) {
    // User can access their own data or admin can access any data
}
```

---

This cheatsheet provides common patterns used in microservices and Spring Boot applications. These patterns help solve common architectural challenges and promote best practices in distributed systems development.
