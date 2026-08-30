# How do you implement paging and sorting in Spring Data REST? 🟡

## Question

Explain how to implement pagination and sorting in Spring Data REST endpoints.

## Answer

Spring Data REST provides built-in support for pagination and sorting through Spring Data's Pageable and Sort interfaces. Clients can request paginated data using query parameters like page, size, and sort. The response includes metadata about total pages, total elements, and navigation links for next/previous pages.

For custom query methods, you can add Pageable and Sort parameters to enable pagination and sorting. Spring Data REST automatically exposes these parameters and returns HAL-formatted responses with pagination metadata and links for navigation.

## Code Example

```java
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {

    // Paginated query method
    Page<User> findByNameContaining(String name, Pageable pageable);

    // Sorted query method
    List<User> findByActiveTrue(Sort sort);

    // Both paginated and sorted
    Page<User> findByEmailContaining(String email, Pageable pageable);
}

// Example requests:
// GET /api/users?page=0&size=10&sort=name,asc
// GET /api/users/search/byNameContaining?name=John&page=0&size=5
// GET /api/users?sort=name,desc&sort=email,asc
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Data REST's built-in pagination and sorting capabilities and how to leverage them in API design.

## ⚡ Follow-up Questions

1. How do you customize default pagination parameters?
2. What is the difference between Page and Slice in Spring Data?
3. How do you implement custom pagination logic?

## 📚 Related Topics

- [Creating Repositories](./02-creating-repositories.md)
- [Customizing Endpoints](./04-customizing-endpoints.md)

## Navigation

[← Previous Question](./05-hateoas.md) | [Next Question →](./07-event-handling.md)
