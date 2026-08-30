# 03. Spring Data REST

This section covers Spring Data REST for building hypermedia-driven REST repositories with minimal code. Spring Data REST automatically creates REST endpoints for Spring Data repositories, following HATEOAS principles.

## 📋 Prerequisites

- Understanding of Spring Data JPA (from Section 01)
- Knowledge of RESTful API design principles
- Familiarity with database entities and relationships
- Basic understanding of HATEOAS concepts

## 📚 Question Index

| # | Question | Difficulty |
|---|----------|------------|
| 01 | [What is Spring Data REST and how does it work?](./01-what-is-spring-data-rest.md) | 🟢 |
| 02 | [How do you create a Spring Data REST repository?](./02-creating-repositories.md) | 🟢 |
| 03 | [What are repository projections in Spring Data REST?](./03-repository-projections.md) | 🟡 |
| 04 | [How do you customize Spring Data REST endpoints?](./04-customizing-endpoints.md) | 🟡 |
| 05 | [What is HATEOAS and how does Spring Data REST implement it?](./05-hateoas.md) | 🟡 |
| 06 | [How do you implement paging and sorting in Spring Data REST?](./06-paging-sorting.md) | 🟡 |
| 07 | [How do you handle events in Spring Data REST?](./07-event-handling.md) | 🟡 |
| 08 | [What are the different ways to secure Spring Data REST endpoints?](./08-security.md) | 🟠 |
| 09 | [How do you validate data in Spring Data REST?](./09-validation.md) | 🟡 |
| 10 | [How do you customize the JSON response in Spring Data REST?](./10-json-customization.md) | 🟠 |
| 11 | [What are the limitations of Spring Data REST?](./11-limitations.md) | 🟡 |
| 12 | [How do you integrate Spring Data REST with custom controllers?](./12-custom-controllers.md) | 🟠 |

## 🎯 Key Takeaways

- **Automatic REST Endpoints**: Spring Data REST automatically creates RESTful endpoints for repositories
- **HATEOAS**: Follows hypermedia principles with discoverable APIs
- **Projections**: Customize response shapes without changing entity structure
- **Paging/Sorting**: Built-in support for pagination and sorting
- **Event Handling**: Hook into repository events for custom logic
- **Integration**: Works alongside custom REST controllers

## 🔗 Related Sections

- [01. Spring Boot Fundamentals](../01-spring-boot-fundamentals/README.md) - Foundation for Spring Data
- [12. Database Patterns](../12-database-patterns/README.md) - Advanced database patterns
- [08. Spring Rest Client](../08-spring-rest-client/README.md) - Consuming REST APIs

## 📖 Resources

- [Spring Data REST Reference](https://docs.spring.io/spring-data/rest/docs/current/reference/html/)
- [Spring Data JPA Documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [HATEOAS Principles](https://restfulapi.net/hateoas/)

## 💡 Study Tips

1. **Start with basics**: Create simple repositories and explore auto-generated endpoints
2. **Understand HATEOAS**: Learn how hypermedia drives API discoverability
3. **Practice projections**: Customize responses for different use cases
4. **Explore events**: Hook into repository lifecycle events
5. **Know limitations**: Understand when to use custom controllers instead

## 🎓 Difficulty Breakdown

- **🟢 Beginner (2 questions)**: Basic concepts and repository creation
- **🟡 Intermediate (7 questions)**: Customization and advanced features
- **🟠 Advanced (3 questions)**: Complex scenarios and integration

Spring Data REST provides rapid development of RESTful APIs but requires understanding of its conventions and limitations for production use.
