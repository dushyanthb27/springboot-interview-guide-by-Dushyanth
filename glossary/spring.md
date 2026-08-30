# Spring Ecosystem Terminology

A comprehensive glossary of Spring Framework and Spring Boot specific terms and concepts.

## A

### @Autowired
Annotation used for automatic dependency injection in Spring, can be applied to constructors, fields, and setter methods.

### @ApplicationScoped
Annotation indicating that a bean lives for the entire lifecycle of the application (singleton scope by default in Spring).

### Actuator
Spring Boot module that provides production-ready features like health checks, metrics, and monitoring endpoints.

### AOP (Aspect-Oriented Programming)
Programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns, implemented in Spring through aspects and advice.

### Advice
Action taken by an aspect at a particular join point in AOP, such as before, after, or around method execution.

## B

### Bean
An object that is instantiated, assembled, and managed by a Spring IoC container.

### @Bean
Annotation used to indicate that a method produces a bean to be managed by the Spring container.

### @BeanProperty
Annotation for JavaBeans properties to indicate getters and setters for Spring Data binding.

### Bootstrapping
Process of initializing a Spring application context, typically done through Spring Boot's SpringApplication class.

### @Builder
Annotation from Project Lombok that generates a builder pattern for creating object instances.

## C

### @Component
Generic annotation for any Spring-managed component, serves as a meta-annotation for other stereotypes like @Service, @Repository, @Controller.

### @Configuration
Annotation indicating that a class declares one or more @Bean methods and may be processed by the Spring container.

### @Conditional
Family of annotations for conditional bean registration based on various conditions (class presence, property values, etc.).

### @ConfigurationProperties
Annotation for binding external configuration properties to a strongly-typed bean.

### Context
The Spring ApplicationContext, which provides configuration for the application and manages beans.

### @Controller
Annotation indicating that a class is a Spring MVC controller, typically used in web applications.

### Cross-Origin Resource Sharing (CORS)
Security feature that Spring Security can configure to allow or restrict cross-origin requests.

### @CrossOrigin
Annotation used to enable cross-origin requests on specific controller methods.

## D

### @Data
Annotation from Project Lombok that generates getters, setters, toString, equals, and hashCode methods.

### @DataJpaTest
Annotation for testing JPA repositories, configures only the relevant parts of the application context.

### @DataMongoTest
Annotation for testing MongoDB repositories, similar to @DataJpaTest but for MongoDB.

### Dependency Injection
Design pattern where dependencies are provided to a class rather than the class creating them, core concept in Spring.

### @DependsOn
Annotation specifying that a bean depends on other beans being initialized first.

### @Deprecated
Annotation indicating that a particular element (class, method, field) is deprecated and should no longer be used.

### DTO (Data Transfer Object)
Pattern for transferring data between processes, commonly used in Spring applications for API responses.

## E

### @EnableAutoConfiguration
Annotation that enables Spring Boot's auto-configuration mechanism (part of @SpringBootApplication).

### @EnableDiscoveryClient
Annotation that enables service discovery in Spring Cloud applications.

### @EnableEurekaClient
Annotation that enables Eureka client functionality for service registration and discovery.

### @EnableGlobalMethodSecurity
Annotation that enables method-level security in Spring Security.

### @EnableWebSecurity
Annotation that enables Spring Security web security configuration.

### @EnableWebFlux
Annotation that enables Spring WebFlux reactive web framework.

### @EnableWebMvc
Annotation that enables Spring MVC configuration.

### Entity
JPA annotation marking a class as a persistent entity, mapped to a database table.

### @Entity
JPA annotation indicating that a class is an entity and should be mapped to a database table.

### @ExceptionHandler
Annotation for handling exceptions in Spring MVC controllers.

## F

### @FeignClient
Annotation for declarative HTTP clients in Spring Cloud OpenFeign.

### FactoryBean
Interface for objects that are themselves factories for other beans, used for complex bean creation logic.

### @Filter
Annotation for registering servlet filters in Spring applications.

### @FunctionalInterface
Annotation indicating that an interface is intended to be a functional interface (single abstract method).

## G

### @GetMapping
Annotation for mapping HTTP GET requests to handler methods.

### @GeneratedValue
JPA annotation for specifying the strategy for generating primary key values.

### @GlobalExceptionHandler
Annotation for handling exceptions globally across the application.

## H

### @HystrixCommand
Annotation for circuit breaker pattern implementation using Netflix Hystrix (deprecated in favor of Resilience4j).

### @Header
Annotation for mapping HTTP headers to method parameters in Spring MVC.

### @Headers
Annotation for specifying HTTP headers in declarative REST clients.

## I

### @Id
JPA annotation marking a field as the primary key of an entity.

### @Import
Annotation for importing additional configuration classes.

### @ImportResource
Annotation for importing XML configuration files into Spring context.

### @Inject
Annotation from JSR-330 for dependency injection, alternative to @Autowired.

### @Inheritance
JPA annotation for specifying inheritance strategies in entity hierarchies.

### @InitBinder
Annotation for customizing data binding in Spring MVC controllers.

### @Interceptor
Annotation for registering interceptors in Spring MVC.

## J

### @JoinColumn
JPA annotation for specifying the join column in a relationship.

### @JoinTable
JPA annotation for specifying the join table in many-to-many relationships.

### JPA (Java Persistence API)
Java specification for object-relational mapping, implemented by Spring Data JPA.

### JTA (Java Transaction API)
Java API for managing distributed transactions in Java EE applications.

### @JsonAlias
Jackson annotation for specifying alternative names for JSON properties.

### @JsonIgnore
Jackson annotation for ignoring properties during JSON serialization/deserialization.

### @JsonProperty
Jackson annotation for customizing property names in JSON.

## K

### @Keycloak
Annotation for Keycloak integration in Spring Security.

## L

### @Lazy
Annotation indicating that a bean should be lazily initialized (only when first requested).

### @Lob
JPA annotation for large object fields (BLOB/CLOB).

### @LocalDateTime
Annotation for binding LocalDateTime parameters in Spring MVC.

### @Lock
JPA annotation for specifying lock modes when accessing entities.

## M

### @Mapper
Annotation from MyBatis for marking interfaces as mapper interfaces.

### @ManyToMany
JPA annotation for many-to-many relationships between entities.

### @ManyToOne
JPA annotation for many-to-one relationships between entities.

### @MessageMapping
Annotation for mapping messages to handler methods in Spring Messaging.

### @ModelAttribute
Annotation for binding model attributes in Spring MVC.

### @Mapping
Annotation for customizing field mappings in MapStruct.

### @MeterRegistry
Annotation for registering custom metrics in Spring Boot Actuator.

### @Microservice
Conceptual term for small, autonomous services in microservices architecture.

### @MockBean
Annotation for creating mock beans in Spring Boot tests.

### @MockMvc
Annotation for setting up MockMvc in Spring Boot tests.

### @MongoId
Annotation for MongoDB document identifiers.

### @ManyToOne
JPA annotation for many-to-one relationships.

## N

### @NoArgsConstructor
Annotation from Project Lombok for generating a no-arguments constructor.

### @NotNull
Annotation from Bean Validation for validating that a field is not null.

## O

### @OneToMany
JPA annotation for one-to-many relationships between entities.

### @OneToOne
JPA annotation for one-to-one relationships between entities.

### @ApiOperation
Swagger annotation for documenting API operations.

### @Order
Annotation for specifying the order of bean initialization or aspect execution.

### @Override
Java annotation indicating that a method overrides a method from a superclass.

## P

### @PathVariable
Annotation for mapping URI template variables to method parameters.

### @PostMapping
Annotation for mapping HTTP POST requests to handler methods.

### @Primary
Annotation indicating that a bean should be preferred when multiple beans of the same type exist.

### @Profile
Annotation for specifying which Spring profiles a bean or configuration belongs to.

### @PropertySource
Annotation for loading property files into the Spring environment.

### @PutMapping
Annotation for mapping HTTP PUT requests to handler methods.

### @PatchMapping
Annotation for mapping HTTP PATCH requests to handler methods.

### @PreAuthorize
Spring Security annotation for method-level security with pre-invocation authorization checks.

### @PostAuthorize
Spring Security annotation for method-level security with post-invocation authorization checks.

### @PostFilter
Spring Security annotation for filtering returned collections based on security rules.

### @PreFilter
Spring Security annotation for filtering method parameters based on security rules.

## Q

### @Query
JPA annotation for defining custom JPQL or native SQL queries.

### @Qualifier
Annotation for disambiguating beans when multiple beans of the same type exist.

## R

### @Repository
Annotation indicating that a class is a repository (data access object), enables exception translation.

### @RepositoryRestResource
Spring Data REST annotation for customizing repository export.

### @RequestMapping
General annotation for mapping HTTP requests to handler methods.

### @RequestBody
Annotation for binding HTTP request body to a method parameter.

### @ResponseBody
Annotation for indicating that a method return value should be written directly to the HTTP response body.

### @RestController
Combination of @Controller and @ResponseBody, commonly used for REST APIs.

### @RestControllerAdvice
Annotation for global exception handling in REST controllers.

### @Retryable
Annotation for retrying failed operations using Spring Retry.

### @RabbitListener
Annotation for creating message listeners in Spring AMQP.

### @RabbitHandler
Annotation for handling specific message types in a @RabbitListener.

### @Reactive
General term for reactive programming in Spring (WebFlux, Reactor).

### @RequestMapping
General annotation for mapping HTTP requests to handler methods.

## S

### @Service
Annotation indicating that a class is a service layer component.

### @Scheduled
Annotation for scheduling tasks at fixed intervals or cron expressions.

### @Scope
Annotation for specifying the scope of a bean (singleton, prototype, etc.).

### @Secured
Spring Security annotation for method-level security.

### @Size
Bean Validation annotation for validating the size of collections, strings, or arrays.

### @SpringBootApplication
Core annotation that combines @Configuration, @EnableAutoConfiguration, and @ComponentScan.

### @SpringBootTest
Annotation for integration testing in Spring Boot, loads the full application context.

### @SpringConfiguration
Annotation indicating that a class is a Spring configuration class.

### @SpyBean
Annotation for creating spy beans in Spring Boot tests.

### @SneakyThrows
Lombok annotation for sneaky throwing of checked exceptions.

### @Sql
Annotation for executing SQL scripts before or after test methods.

### @SqlGroup
Annotation for grouping multiple @Sql annotations.

### @Scheduled
Annotation for scheduling tasks.

## T

### @Test
JUnit annotation for marking a method as a test method.

### @TestConfiguration
Annotation for test-specific configuration classes.

### @TestPropertySource
Annotation for loading test-specific property files.

### @Transactional
Annotation for defining transaction boundaries on methods or classes.

### @Transient
JPA annotation indicating that a field should not be persisted.

### @Timed
Annotation for timing method execution in Spring Boot Actuator.

### @ToString
Lombok annotation for generating toString method.

### @Transactional
Annotation for transaction management.

## U

### @UniqueConstraint
JPA annotation for specifying unique constraints on entity fields.

### @Update
Annotation for update operations in MyBatis.

### @Upload
Conceptual term for file upload functionality in Spring MVC.

## V

### @Value
Annotation for injecting property values into beans.

### @Valid
Bean Validation annotation for triggering validation of method parameters.

### @Validated
Spring annotation for enabling method-level validation.

### @Value
Annotation for injecting property values.

## W

### @WebMvcTest
Annotation for testing Spring MVC controllers, configures only the web layer.

### @WebFluxTest
Annotation for testing Spring WebFlux controllers.

### @WebFilter
Annotation for registering servlet filters.

### @WebListener
Annotation for registering servlet listeners.

### @WebServlet
Annotation for registering servlets.

### @WithMockUser
Spring Security test annotation for running tests with a mock user.

### @WithUserDetails
Spring Security test annotation for running tests with specific user details.

### @WebMvcTest
Annotation for testing Spring MVC controllers.

## X

### @XmlRootElement
JAXB annotation for XML serialization.

### @XmlElement
JAXB annotation for XML element mapping.

## Y

### @Yarn
Conceptual term for Apache Yarn integration in Spring (less common).

## Z

### @Zipkin
Conceptual term for Zipkin distributed tracing integration in Spring Cloud Sleuth.

---

This glossary covers the most common Spring Framework and Spring Boot annotations and concepts you'll encounter in interviews and daily development. Understanding these terms is essential for effective communication about Spring-based applications.
