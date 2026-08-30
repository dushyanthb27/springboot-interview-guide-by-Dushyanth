# How do you test resilience patterns? 🟠

## Question

How do you test circuit breakers, retries, and other resilience patterns?

## Answer

Testing resilience patterns involves simulating failures to verify patterns work correctly. This can be done using mocking frameworks to simulate service failures, delaying responses to test timeouts, and returning errors to test circuit breakers and retries. Spring Boot Test provides tools for testing resilience patterns.

Testing approaches include using @MockBean to simulate failures, Testcontainers for integration testing with real services, and custom test utilities to inject failures. Tests should verify circuit breaker state transitions, retry behavior, and fallback execution.

## Code Example

```java
@SpringBootTest
@AutoConfigureMockMvc
public class ResilienceTest {

    @MockBean
    private UserServiceClient userServiceClient;

    @Test
    public void testCircuitBreaker() {
        when(userServiceClient.getUser(any()))
                .thenThrow(new ServiceUnavailableException("Service down"))
                .thenReturn(new User(1L, "John"));

        User user = userServiceClient.getUser(1L);
        assertNull(user); // Circuit breaker should be open

        // Wait for circuit breaker to enter half-open state
        Thread.sleep(11000);

        user = userServiceClient.getUser(1L);
        assertNotNull(user); // Should succeed in half-open state
    }

    @Test
    public void testRetry() {
        when(userServiceClient.getUser(any()))
                .thenThrow(new IOException())
                .thenThrow(new IOException())
                .thenReturn(new User(1L, "John"));

        User user = userServiceClient.getUser(1L);
        assertNotNull(user); // Should succeed after retries
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of testing strategies for resilience patterns and your ability to verify they work correctly.

## ⚠️ Common Mistake
Not testing resilience patterns thoroughly, leading to unexpected behavior in production.

## ⚡ Follow-up Questions

1. How do you simulate network failures in tests?
2. What is the difference between unit and integration testing for resilience?
3. How do you test circuit breaker state transitions?

## 📚 Related Topics

- [Circuit Breaker Pattern](./02-circuit-breaker-pattern.md)
- [Retry Patterns](./03-retry-patterns.md)
- [13. Testing Strategies](../13-testing-strategies/README.md)

## Navigation

[← Previous Question](./09-anti-patterns.md) | [Next Question →](./11-spring-cloud-integration.md)
