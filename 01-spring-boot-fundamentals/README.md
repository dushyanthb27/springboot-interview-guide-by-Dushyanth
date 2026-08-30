# 01. Spring Boot Fundamentals

This section covers the fundamental concepts of Spring Boot, including auto-configuration, dependency injection, application structure, and core annotations. These are the building blocks for all Spring Boot applications and microservices.

## 📋 Prerequisites

- Basic knowledge of Java programming
- Understanding of object-oriented programming concepts
- Familiarity with build tools (Maven or Gradle)
- Basic understanding of web applications

## 📚 Question Index

| # | Question | Difficulty |
|---|----------|------------|
| 01 | [What is Spring Boot and why would you use it?](./01-what-is-spring-boot.md) | 🟢 |
| 02 | [How does Spring Boot auto-configuration work?](./02-auto-configuration.md) | 🟡 |
| 03 | [What is the difference between @Component, @Service, @Repository, and @Controller?](./03-stereotype-annotations.md) | 🟢 |
| 04 | [Explain dependency injection in Spring Boot](./04-dependency-injection.md) | 🟡 |
| 05 | [What are Spring Boot starters and how do they work?](./05-spring-boot-starters.md) | 🟢 |
| 06 | [How do you configure application properties in Spring Boot?](./06-application-properties.md) | 🟡 |
| 07 | [What is the difference between @Autowired and constructor injection?](./07-dependency-injection-methods.md) | 🟡 |
| 08 | [Explain the Spring Boot application lifecycle](./08-application-lifecycle.md) | 🟠 |
| 09 | [How do you create and use custom Spring Boot annotations?](./09-custom-annotations.md) | 🟠 |
| 10 | [What are profiles in Spring Boot and how do you use them?](./10-profiles.md) | 🟡 |
| 11 | [How do you handle exceptions in Spring Boot applications?](./11-exception-handling.md) | 🟡 |
| 12 | [What is the purpose of @SpringBootApplication annotation?](./12-springboot-application.md) | 🟢 |
| 13 | [How do you configure beans in Spring Boot?](./13-bean-configuration.md) | 🟡 |
| 14 | [What is the difference between singleton and prototype scope?](./14-bean-scopes.md) | 🟠 |
| 15 | [How do you implement conditional bean loading in Spring Boot?](./15-conditional-beans.md) | 🟠 |

## 🎯 Key Takeaways

- **Auto-configuration**: Spring Boot automatically configures applications based on dependencies
- **Dependency Injection**: Core IoC pattern for loose coupling and testability
- **Starter Dependencies**: Simplified dependency management for common use cases
- **Profiles**: Environment-specific configuration management
- **Bean Scopes**: Understanding singleton vs prototype for proper bean lifecycle
- **Configuration**: Multiple ways to configure beans (annotations, Java config, properties)

## 🔗 Related Sections

- [02. Microservices Architecture](../02-microservices-architecture/README.md) - Builds on Spring Boot fundamentals for distributed systems
- [08. Spring Rest Client](../08-spring-rest-client/README.md) - Uses Spring Boot fundamentals for REST communication
- [13. Testing Strategies](../13-testing-strategies/README.md) - Testing Spring Boot applications

## 📖 Resources

- [Spring Boot Official Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/)
- [Spring Boot Start](https://start.spring.io/)

## 💡 Study Tips

1. **Start with basics**: Master the core annotations and concepts first
2. **Practice coding**: Create simple Spring Boot applications to test concepts
3. **Understand the "why"**: Don't just memorize, understand the reasoning
4. **Read the source**: Look at Spring Boot source code for deeper understanding
5. **Build incrementally**: Start simple and add complexity gradually

## 🎓 Difficulty Breakdown

- **🟢 Beginner (4 questions)**: Basic concepts and annotations
- **🟡 Intermediate (7 questions)**: Configuration and dependency injection
- **🟠 Advanced (4 questions)**: Advanced patterns and lifecycle management

This section provides the foundation for all subsequent sections. Master these concepts before moving to more advanced microservices topics.
