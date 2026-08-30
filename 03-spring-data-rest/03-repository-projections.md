# What are repository projections in Spring Data REST? 🟡

## Question

Explain repository projections in Spring Data REST and when you would use them.

## Answer

Repository projections allow you to customize the shape of data returned by Spring Data REST without modifying the entity structure. Projections are interfaces that define a subset of fields to return, providing lightweight responses and avoiding over-fetching data. They're useful for API optimization, security (hiding sensitive fields), and providing different views of the same data.

Spring Data REST supports interface-based projections (using @Projection annotation) and DTO-based projections. Projections can be used with repository methods and are automatically exposed as REST endpoints. You can also use excerpts to include projections in collection resources.

## Code Example

```java
package com.example.demo.projection;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.data.rest.core.annotation.Projection;

// Interface-based projection
@Projection(name = "summary", types = User.class)
public interface UserSummary {
    Long getId();
    String getName();
}

@Projection(name = "detail", types = User.class)
public interface UserDetail {
    Long getId();
    String getName();
    String getEmail();
    String getPhone();
}

// Projection with computed properties
@Projection(name = "withStatus", types = User.class)
public interface UserWithStatus {
    Long getId();
    String getName();

    @Value("#{target.active ? 'ACTIVE' : 'INACTIVE'}")
    String getStatus();
}

// Repository using projections
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {

    // Projection endpoint: GET /users/search/byEmail?email=xxx&projection=summary
    UserSummary findByEmail(@Param("email") String email);

    // Collection projection: GET /users?projection=summary
    Page<UserSummary> findAllProjectedBy(Pageable pageable);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of data shaping techniques and your ability to optimize API responses. They're looking for awareness of performance and security benefits of projections.

## ⚠️ Common Mistake
Using projections for complex business logic instead of simple data shaping. Another mistake is not understanding that projections are read-only views, not for data modification.

## ⚡ Follow-up Questions

1. How do you use DTOs as projections in Spring Data REST?
2. What is the difference between interface-based and DTO-based projections?
3. How do you apply projections to collection resources?

## 📚 Related Topics

- [Customizing Endpoints](./04-customizing-endpoints.md)
- [JSON Customization](./10-json-customization.md)
- [Security](./08-security.md)

## Navigation

[← Previous Question](./02-creating-repositories.md) | [Next Question →](./04-customizing-endpoints.md)
