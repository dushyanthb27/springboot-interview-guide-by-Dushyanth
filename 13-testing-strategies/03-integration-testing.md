# How do you write integration tests with Spring Boot Test? 🟡

## Question

How do you write integration tests using Spring Boot Test annotations?

## Answer

Spring Boot Test provides @SpringBootTest for full application context integration tests, @WebMvcTest for MVC layer testing, @DataJpaTest for repository testing, and @JsonTest for JSON serialization testing. Integration tests verify components work together with real infrastructure.

Configuration involves using appropriate test slice annotations, configuring test properties, and using @TestConfiguration for test-specific beans. Integration tests are slower than unit tests but provide more comprehensive testing.

## Code Example

```java
@SpringBootTest
@AutoConfigureMockMvc
class OrderControllerIntegrationTest {
    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private OrderRepository orderRepository;

    @Test
    void testCreateOrder() throws Exception {
        mockMvc.perform(post("/api/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"userId\":1,\"amount\":100}"))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.id").exists());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Boot Test annotations and your ability to write integration tests.

## ⚠️ Common Mistake
Using @SpringBootTest for simple unit tests, making tests unnecessarily slow.

## ⚡ Follow-up Questions

1. What is the difference between @SpringBootTest and test slice annotations?
2. How do you configure test properties?
3. How do you use @TestConfiguration?

## 📚 Related Topics

- [Unit Testing](./02-unit-testing.md)
- [Test Slices](./08-test-slices.md)
- [Testcontainers](./06-testcontainers.md)

## Navigation

[← Previous Question](./02-unit-testing.md) | [Next Question →](./04-rest-api-testing.md)
