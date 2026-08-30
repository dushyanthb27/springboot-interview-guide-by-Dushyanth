# How do you implement test slices? 🟡

## Question

What are test slices in Spring Boot Test and when should you use them?

## Answer

Test slices are focused test annotations that load only a subset of the application context for testing specific layers. They include @WebMvcTest (MVC layer), @WebFluxTest (WebFlux layer), @DataJpaTest (JPA repositories), @JsonTest (JSON serialization), and @DataRedisTest (Redis). Test slices are faster than @SpringBootTest as they load only necessary beans.

Test slices should be used for testing specific layers in isolation, providing faster test execution and more focused testing. They are ideal for unit testing of controllers, repositories, and other components.

## Code Example

```java
@WebMvcTest(OrderController.class)
class OrderControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private OrderService orderService;

    @Test
    void testGetOrder() throws Exception {
        when(orderService.getOrder(1L)).thenReturn(new Order(1L, 100));

        mockMvc.perform(get("/api/orders/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id").value(1));
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of test slices and your ability to choose the right test slice for the scenario.

## ⚠️ Common Mistake
Using @SpringBootTest when a test slice would be more appropriate, making tests slower than necessary.

## ⚡ Follow-up Questions

1. What test slices are available in Spring Boot Test?
2. How do test slices differ from @SpringBootTest?
3. When would you use @SpringBootTest over test slices?

## 📚 Related Topics

- [Integration Testing](./03-integration-testing.md)
- [Unit Testing](./02-unit-testing.md)
- [REST API Testing](./04-rest-api-testing.md)

## Navigation

[← Previous Question](./07-messaging-testing.md) | [Next Question →](./09-security-testing.md)
