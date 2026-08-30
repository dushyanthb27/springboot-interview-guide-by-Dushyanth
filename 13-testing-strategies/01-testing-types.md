# What are the different types of testing? 🟢

## Question

What are the different types of testing in software development and when to use each?

## Answer

Testing types include unit testing (testing individual components in isolation), integration testing (testing component interactions), end-to-end testing (testing complete workflows), contract testing (testing API compatibility), performance testing (testing system performance), and security testing (testing security vulnerabilities).

Unit tests are fast and test logic in isolation. Integration tests verify components work together. E2E tests test complete user flows. Contract tests ensure API compatibility between services. Performance tests verify system can handle load. Security tests identify vulnerabilities.

## Code Example

```java
// Unit test example
@Test
public void testCalculateTotal() {
    OrderService service = new OrderService();
    Order order = new Order();
    order.setAmount(100);
    assertEquals(110, service.calculateTotal(order));
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of testing types and your ability to choose the right testing approach.

## ⚡ Follow-up Questions

1. What is the testing pyramid?
2. When would you use integration tests vs unit tests?
3. What is the difference between E2E and integration testing?

## 📚 Related Topics

- [Unit Testing](./02-unit-testing.md)
- [Integration Testing](./03-integration-testing.md)
- [Microservices Testing](./11-microservices-testing.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-unit-testing.md)
