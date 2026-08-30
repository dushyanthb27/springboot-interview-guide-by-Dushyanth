# How do you write unit tests with JUnit and Mockito? 🟡

## Question

How do you write unit tests using JUnit 5 and Mockito in Spring Boot applications?

## Answer

Unit tests with JUnit 5 use @Test annotation for test methods, @BeforeEach for setup, and assertions for verification. Mockito is used to mock dependencies with @Mock and @InjectMocks annotations. Unit tests should be fast, isolated, and test specific logic without external dependencies.

Implementation involves creating test classes, mocking dependencies, setting up test data, calling the method under test, and asserting expected results using JUnit assertions.

## Code Example

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
    @Mock
    private OrderRepository orderRepository;

    @Mock
    private UserServiceClient userServiceClient;

    @InjectMocks
    private OrderService orderService;

    @Test
    void testCreateOrder() {
        // Arrange
        Order order = new Order();
        order.setUserId(1L);
        when(orderRepository.save(any())).thenReturn(order);
        when(userServiceClient.getUser(1L)).thenReturn(new User(1L, "John"));

        // Act
        Order result = orderService.createOrder(order);

        // Assert
        assertNotNull(result);
        verify(orderRepository).save(order);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of unit testing and your ability to use JUnit and Mockito effectively.

## ⚠️ Common Mistake
Not isolating unit tests properly, leading to slow tests that depend on external systems.

## ⚡ Follow-up Questions

1. What is the difference between @Mock and @Spy?
2. How do you verify method calls with Mockito?
3. What are the AAA (Arrange-Act-Assert) pattern?

## 📚 Related Topics

- [Integration Testing](./03-integration-testing.md)
- [Test Slices](./08-test-slices.md)
- [Testing Best Practices](./13-testing-best-practices.md)

## Navigation

[← Previous Question](./01-testing-types.md) | [Next Question →](./03-integration-testing.md)
