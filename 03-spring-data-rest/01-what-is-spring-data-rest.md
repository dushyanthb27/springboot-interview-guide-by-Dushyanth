# What is Spring Data REST and how does it work? 🟢

## Question

What is Spring Data REST and what are its main features and benefits?

## Answer

Spring Data REST is a Spring framework module that automatically exposes Spring Data repositories as RESTful endpoints with minimal configuration. It builds on top of Spring Data JPA, MongoDB, and other Spring Data modules to create hypermedia-driven REST APIs that follow HATEOAS (Hypermedia as the Engine of Application State) principles.

Main features include automatic CRUD endpoint generation, support for pagination, sorting, and filtering, HATEOAS-compliant responses with discoverable links, support for projections and excerpts, and integration with Spring Data repositories. Benefits include rapid API development, consistent API structure, reduced boilerplate code, and built-in support for REST best practices.

## Code Example

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;

@SpringBootApplication
public class DataRestApplication {
    public static void main(String[] args) {
        SpringApplication.run(DataRestApplication.class, args);
    }
}

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;

    // Getters and setters
}

@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends JpaRepository<User, Long> {
    // Spring Data REST automatically creates REST endpoints:
    // GET /users - Get all users
    // POST /users - Create a user
    // GET /users/{id} - Get a specific user
    // PUT /users/{id} - Update a user
    // PATCH /users/{id} - Partially update a user
    // DELETE /users/{id} - Delete a user
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Data REST's purpose and its automatic endpoint generation capabilities. They're looking for awareness of when to use this framework versus custom REST controllers.

## ⚡ Follow-up Questions

1. What are the limitations of Spring Data REST?
2. How does Spring Data REST differ from custom REST controllers?
3. What is HATEOAS and why is it important?

## 📚 Related Topics

- [Creating Repositories](./02-creating-repositories.md)
- [HATEOAS](./05-hateoas.md)
- [01. Spring Boot Fundamentals](../01-spring-boot-fundamentals/README.md)

## Navigation

[← Main Section](./README.md) | [Next Question →](./02-creating-repositories.md)
