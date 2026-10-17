# What are the different types of testing? 🟢

## Question

What are the different types of testing in software development and when to use each? Explain the testing pyramid and how it guides test strategy.

## Answer

Testing types in software development, organized by the testing pyramid:

**1. Unit Tests (Bottom of Pyramid - Largest Volume)**
- Test individual components (classes, methods) in isolation
- Fast execution (milliseconds), no external dependencies
- Test business logic, edge cases, and error handling
- Use mocks/stubs to isolate dependencies
- Should comprise 70-80% of your test suite

**2. Integration Tests (Middle of Pyramid - Medium Volume)**
- Test interactions between components and external systems
- Slower than unit tests (seconds), may use real dependencies
- Test database integration, external API calls, message queues
- Use Testcontainers for real databases, services in Docker
- Should comprise 15-20% of your test suite

**3. End-to-End (E2E) Tests (Top of Pyramid - Smallest Volume)**
- Test complete user workflows across the entire system
- Slowest execution (minutes), most expensive to maintain
- Test user journeys from UI to database and back
- Use tools like Selenium, Cypress, or Playwright
- Should comprise 5-10% of your test suite

**4. Contract Tests**
- Test API compatibility between services
- Ensure consumer and provider agree on API contract
- Use tools like Pact or Spring Cloud Contract
- Prevent breaking changes in distributed systems

**5. Performance Tests**
- Test system performance under load
- Include load testing, stress testing, soak testing
- Use tools like JMeter, Gatling, or k6
- Identify bottlenecks, verify SLAs

**6. Security Tests**
- Test for security vulnerabilities
- Include penetration testing, dependency scanning
- Use tools like OWASP ZAP, Snyk
- Identify and fix security issues early

**Testing Pyramid Philosophy:**
- More tests at lower levels (unit tests) because they're fast, cheap, and pinpoint issues
- Fewer tests at higher levels (E2E) because they're slow, expensive, and hard to debug
- Inverted pyramid (many E2E tests) leads to slow, brittle test suites

## Code Example

```java
// Unit Test - fast, isolated, mocks dependencies
@SpringBootTest
class OrderServiceTest {

    @Mock
    private UserRepository userRepository;

    @Mock
    private PaymentService paymentService;

    @InjectMocks
    private OrderService orderService;

    @Test
    void calculateTotal_shouldIncludeTax() {
        // Given
        Order order = new Order();
        order.setAmount(100);
        order.setTaxRate(0.10);

        // When
        BigDecimal total = orderService.calculateTotal(order);

        // Then
        assertEquals(new BigDecimal("110.00"), total);
    }
}

// Integration Test - tests real database interaction
@SpringBootTest
@Testcontainers
class OrderRepositoryIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:14");

    @Autowired
    private OrderRepository orderRepository;

    @Test
    void saveOrder_shouldPersistToDatabase() {
        // Given
        Order order = new Order();
        order.setAmount(100);

        // When
        Order saved = orderRepository.save(order);

        // Then
        assertNotNull(saved.getId());
        assertEquals(100, saved.getAmount());
    }
}

// E2E Test - tests complete user flow
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@AutoConfigureWebClient
class OrderE2ETest {

    @Autowired
    private WebTestClient webClient;

    @Test
    void createOrder_completeWorkflow() {
        // Create order via API
        Order order = webClient.post()
            .uri("/api/orders")
            .bodyValue(new OrderRequest(100, "user123"))
            .exchange()
            .expectStatus().isCreated()
            .expectBody(Order.class)
            .returnResult()
            .getResponseBody();

        // Verify order was saved
        assertNotNull(order.getId());
        assertEquals(100, order.getAmount());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of testing types and your ability to choose the right testing approach. They're looking for knowledge of the testing pyramid and when to use each type of test. A strong answer demonstrates that you understand the trade-offs between test speed, cost, and coverage.

## ⚠️ Common Mistake

Focusing too much on E2E tests because they seem more "comprehensive." This leads to slow, brittle test suites that are expensive to maintain. Another mistake is not testing at the right level—for example, using integration tests when unit tests would suffice, or vice versa. Understanding the testing pyramid is crucial for efficient test strategy.

## ⚡ Follow-up Questions

1. What is the testing pyramid, and why should you have more unit tests than E2E tests?
2. When would you use integration tests vs unit tests? Give specific examples.
3. What is the difference between E2E and integration testing?
4. How do you decide when to mock a dependency vs use the real implementation?
5. What is Testcontainers, and how does it improve integration testing?

## 📚 Related Topics

- [Unit Testing](./02-unit-testing.md)
- [Integration Testing](./03-integration-testing.md)
- [Microservices Testing](./11-microservices-testing.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-unit-testing.md)
