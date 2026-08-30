# How do you test REST APIs? 🟡

## Question

How do you test REST APIs in Spring Boot applications?

## Answer

REST API testing is done using MockMvc for controller testing or TestRestTemplate for integration testing. MockMvc allows testing controllers without starting a full HTTP server, while TestRestTemplate tests with a real HTTP server. Both approaches can test request/response handling, status codes, and JSON serialization.

Implementation involves using @WebMvcTest for controller testing or @SpringBootTest with @AutoConfigureMockMvc, performing HTTP requests, and asserting response status, headers, and body.

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
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.amount").value(100));
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of REST API testing and your ability to use MockMvc effectively.

## ⚠️ Common Mistake
Not testing error scenarios, leading to incomplete API test coverage.

## ⚡ Follow-up Questions

1. How do you test POST requests with MockMvc?
2. What is the difference between MockMvc and TestRestTemplate?
3. How do you test error responses?

## 📚 Related Topics

- [Integration Testing](./03-integration-testing.md)
- [Test Slices](./08-test-slices.md)
- [Contract Testing](./10-contract-testing.md)

## Navigation

[← Previous Question](./03-integration-testing.md) | [Next Question →](./05-repository-testing.md)
