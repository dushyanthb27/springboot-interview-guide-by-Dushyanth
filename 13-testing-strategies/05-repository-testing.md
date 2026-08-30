# How do you test repositories with Spring Data JPA? 🟡

## Question

How do you test Spring Data JPA repositories?

## Answer

Spring Data JPA repositories are tested using @DataJpaTest annotation, which configures an embedded database and scans for repository interfaces. This test slice provides a lightweight way to test repository functionality without loading the full application context.

Configuration involves using @DataJpaTest, optionally configuring the embedded database (H2 by default), and using TestEntityManager for entity manipulation in tests.

## Code Example

```java
@DataJpaTest
class OrderRepositoryTest {
    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private TestEntityManager entityManager;

    @Test
    void testFindById() {
        Order order = new Order();
        order.setAmount(100);
        entityManager.persist(order);

        Optional<Order> found = orderRepository.findById(order.getId());
        assertTrue(found.isPresent());
        assertEquals(100, found.get().getAmount());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of repository testing and your ability to use @DataJpaTest.

## ⚠️ Common Mistake
Using @SpringBootTest for repository tests, which is slower and unnecessary.

## ⚡ Follow-up Questions

1. What embedded database does @DataJpaTest use by default?
2. How do you configure a different database for tests?
3. What is TestEntityManager?

## 📚 Related Topics

- [Integration Testing](./03-integration-testing.md)
- [Test Slices](./08-test-slices.md)
- [Testcontainers](./06-testcontainers.md)

## Navigation

[← Previous Question](./04-rest-api-testing.md) | [Next Question →](./06-testcontainers.md)
