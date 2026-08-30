# How do you decompose a monolith into microservices? 🟠

## Question

What are the strategies and approaches for decomposing a monolithic application into microservices, and what are the key considerations?

## Answer

Monolith decomposition strategies include: Strangler Fig pattern (gradually replace monolith functionality with microservices by creating a facade that routes requests), Domain-Driven Design (DDD) bounded contexts (identify business domains and align services with them), database decomposition (split shared database into service-specific databases), and vertical slicing (cut functionality by business capability rather than technical layers).

Key considerations include identifying service boundaries based on business capabilities, managing data migration and consistency during transition, handling shared functionality and libraries, maintaining backward compatibility during transition, and choosing the right migration pace (big bang vs incremental). The decomposition should be driven by business needs and team structure rather than technical convenience.

## Code Example

```java
// Strangler Fig Pattern - API Gateway for gradual migration
@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                // Route to new microservice
                .route("user-service", r -> r
                        .path("/api/users/**")
                        .uri("lb://user-service"))
                // Route to monolith for remaining functionality
                .route("monolith", r -> r
                        .path("/api/**")
                        .uri("lb://monolith-service"))
                .build();
    }
}

// Domain-Driven Design Bounded Contexts
// User Bounded Context
@DomainContext("user-management")
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    // User-specific fields only
}

// Order Bounded Context
@DomainContext("order-management")
@SpringBootApplication
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

@Entity
@Table(name = "orders")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long userId; // Reference to user, not full user object
    private BigDecimal amount;
    // Order-specific fields only
}

// Data Migration Strategy
@Service
public class DataMigrationService {
    @Autowired
    private MonolithUserRepository monolithUserRepository;
    @Autowired
    private MicroserviceUserRepository microserviceUserRepository;

    @Scheduled(fixedDelay = 10000)
    public void migrateUsers() {
        List<MonolithUser> users = monolithUserRepository.findUnmigratedUsers();
        users.forEach(monolithUser -> {
            MicroserviceUser microUser = convertToMicroserviceUser(monolithUser);
            microserviceUserRepository.save(microUser);
            monolithUser.setMigrated(true);
            monolithUserRepository.save(monolithUser);
        });
    }

    private MicroserviceUser convertToMicroserviceUser(MonolithUser monolithUser) {
        MicroserviceUser user = new MicroserviceUser();
        user.setName(monolithUser.getName());
        user.setEmail(monolithUser.getEmail());
        return user;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of practical migration strategies and your ability to plan a complex architectural transition. They're looking for awareness of the challenges involved in decomposition and strategies to mitigate risks.

## ⚠️ Common Mistake

Trying to decompose everything at once (big bang approach) which is extremely risky and often fails. Another mistake is decomposing based on technical layers (controllers, services, repositories) rather than business capabilities.

## ⚡ Follow-up Questions

1. How do you handle data consistency during the migration period?
2. What is the Strangler Fig pattern and how does it work?
3. How do you identify the right service boundaries using Domain-Driven Design?

## 📚 Related Topics

- [Service Boundaries](./07-service-boundaries.md)
- [Data Consistency](./05-data-consistency.md)
- [Communication Patterns](./04-communication-patterns.md)

## Navigation

[← Previous Question](./02-benefits-challenges.md) | [Next Question →](./04-communication-patterns.md)
