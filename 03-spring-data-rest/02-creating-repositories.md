# How do you create a Spring Data REST repository? 🟢

## Question

What are the steps to create a Spring Data REST repository and what annotations are used?

## Answer

To create a Spring Data REST repository, you need to define an interface that extends a Spring Data repository interface (like JpaRepository or CrudRepository) and annotate it with @RepositoryRestResource to customize the REST endpoint path and resource relation. The repository should be placed in a package that's scanned by Spring Data REST.

Key annotations include @RepositoryRestResource (customizes the REST resource), @RestResource (hides or customizes specific repository methods), and standard Spring Data annotations like @Query for custom queries. Spring Data REST automatically exposes CRUD operations and custom query methods as REST endpoints.

## Code Example

```java
package com.example.demo.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.repository.query.Param;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;
import org.springframework.data.rest.core.annotation.RestResource;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;

import com.example.demo.model.User;

@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {

    // Custom query method exposed as GET /users/search/byEmail?email=xxx
    User findByEmail(@Param("email") String email);

    // Paginated query exposed as GET /users/search/byNameContaining?name=xxx
    Page<User> findByNameContaining(@Param("name") String name, Pageable pageable);

    // Hidden method - not exposed as REST endpoint
    @RestResource(exported = false)
    User findInternalUser(String email);

    // Custom path for method
    @RestResource(path = "by-email-address")
    User findByEmailAddress(@Param("email") String email);
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your ability to set up Spring Data REST repositories and customize the exposed endpoints. They're looking for understanding of the annotations and how to control what gets exposed.

## ⚡ Follow-up Questions

1. How do you hide specific repository methods from REST exposure?
2. What is the difference between collectionResourceRel and path in @RepositoryRestResource?
3. How do you add custom query methods to Spring Data REST repositories?

## 📚 Related Topics

- [Customizing Endpoints](./04-customizing-endpoints.md)
- [Paging and Sorting](./06-paging-sorting.md)
- [01. Spring Boot Fundamentals](../01-spring-boot-fundamentals/README.md)

## Navigation

[← Previous Question](./01-what-is-spring-data-rest.md) | [Next Question →](./03-repository-projections.md)
