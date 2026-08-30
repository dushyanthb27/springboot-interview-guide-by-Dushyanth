# Microservices Terminology

A comprehensive glossary of microservices architecture terms and concepts.

## A

### API Gateway
A single entry point for all client requests that routes them to appropriate microservices, handles cross-cutting concerns like authentication, rate limiting, and load balancing.

### Asynchronous Communication
Communication pattern where the sender doesn't wait for a response, typically using message queues or event streams for decoupled service interaction.

### Availability
One of the CAP theorem properties - the system guarantees that every request receives a response (success or failure) within a reasonable time frame.

### Autonomy
The ability of a microservice to operate independently, with its own database, deployment pipeline, and team ownership.

## B

### Backend for Frontend (BFF)
A pattern where specific backend services are created for different frontend applications (web, mobile, etc.) to optimize API responses for each client type.

### Bulkhead Pattern
A resilience pattern that isolates resources (like thread pools) to prevent cascading failures when one component experiences issues.

### Bounded Context
A concept from Domain-Driven Design (DDD) that defines the boundaries of a specific domain model, often mapping to a microservice boundary.

## C

### CAP Theorem
A fundamental theorem in distributed systems stating that a distributed data store can only simultaneously provide two out of three guarantees: Consistency, Availability, and Partition Tolerance.

### Circuit Breaker
A pattern that prevents cascading failures by detecting failures and stopping requests to a failing service, allowing it time to recover.

### Client-Side Discovery
Service discovery pattern where clients are responsible for determining the location of service instances, typically using a service registry.

### Cloud-Native
Applications designed to take advantage of cloud computing models, characterized by microservices, containerization, dynamic orchestration, and DevOps practices.

### Consistency
One of the CAP theorem properties - every read receives the most recent write or an error (strong consistency), or reads may return stale data (eventual consistency).

### Containerization
Packaging applications with their dependencies into containers (like Docker) for consistent deployment across different environments.

### Contract Testing
Testing approach that verifies that services adhere to their defined API contracts, ensuring compatibility between services.

## D

### Database per Service
Pattern where each microservice has its own private database, ensuring loose coupling and allowing different data models per service.

### Dead Letter Queue (DLQ)
A queue for messages that couldn't be processed successfully, allowing for later analysis and reprocessing.

### Distributed Transaction
A transaction that spans multiple microservices, requiring coordination across different databases and services.

### Domain-Driven Design (DDD)
An approach to software development that focuses on modeling the business domain and aligning software design with business concepts.

### Docker
A platform for developing, shipping, and running applications in containers, providing isolation and consistency across environments.

## E

### Event-Driven Architecture
Architectural pattern where services communicate through events (messages) rather than direct calls, enabling loose coupling and asynchronous processing.

### Event Sourcing
Pattern where state changes are stored as a sequence of events rather than current state, enabling audit trails and temporal queries.

### Eventual Consistency
Consistency model where updates propagate asynchronously, and the system guarantees that if no new updates are made, all accesses will return the last updated value eventually.

### Externalized Configuration
Pattern where configuration is stored outside the application code, typically in configuration servers or environment variables.

## F

### Fan-Out/Fan-In
Messaging pattern where a message is sent to multiple recipients (fan-out) and responses are aggregated (fan-in).

### Fault Tolerance
The ability of a system to continue operating properly in the event of failure of some of its components.

### Federation
Pattern where multiple independent systems or organizations share resources while maintaining autonomy.

## G

### Gateway Aggregation
Pattern where an API gateway makes multiple service calls and aggregates the results into a single response for the client.

### Graceful Degradation
Design approach where a system continues to operate with reduced functionality when some components fail.

## H

### Health Check
Endpoint or mechanism to monitor the status of a service, typically used by load balancers and orchestrators for routing decisions.

### Horizontal Scaling
Adding more instances of a service to handle increased load, as opposed to vertical scaling (adding resources to a single instance).

### HATEOAS (Hypermedia as the Engine of Application State)
REST architecture constraint where APIs include hypermedia links to related resources, enabling clients to discover available actions dynamically.

## I

### Idempotency
Property of an operation that can be applied multiple times without changing the result beyond the initial application.

### Infrastructure as Code (IaC)
Practice of managing and provisioning infrastructure through machine-readable definition files rather than manual configuration.

### Integration Testing
Testing that verifies different modules or services work together correctly, typically focusing on interfaces and data flow.

## J

### JWT (JSON Web Token)
Compact, URL-safe means of representing claims to be transferred between parties, commonly used for authentication in microservices.

## K

### Kubernetes
Container orchestration platform that automates deployment, scaling, and management of containerized applications.

### Keycloak
Open-source identity and access management solution that provides authentication and authorization services.

## L

### Latency
The time it takes for a request to travel from sender to receiver and back, a critical metric in distributed systems.

### Load Balancing
Distributing incoming network traffic across multiple servers to ensure no single server becomes overwhelmed.

### Loose Coupling
Design principle where components have minimal dependencies on each other, enabling independent development and deployment.

## M

### Microservices
Architectural style that structures an application as a collection of small, autonomous services modeled around business domains.

### Monolith
Traditional architectural style where an application is built as a single, unified unit, typically with a shared codebase and database.

### Message Broker
Intermediate component that receives messages from producers and delivers them to consumers, enabling asynchronous communication.

### Monolithic Architecture
Software architecture where the entire application is built as a single, self-contained unit with shared code and resources.

## N

### Namespace
Mechanism for isolating resources within a system, commonly used in Kubernetes for organizing and securing resources.

### Non-Blocking I/O
I/O model where operations don't block the executing thread, allowing the thread to perform other tasks while waiting for I/O completion.

## O

### OAuth2
Authorization framework that enables applications to obtain limited access to user accounts on an HTTP service.

### Observability
The ability to understand a system's internal state through its external outputs, typically achieved through logging, metrics, and tracing.

### Orchestration
Automated arrangement, coordination, and management of complex computer systems and services.

## P

### Partition Tolerance
One of the CAP theorem properties - the system continues to operate despite network partitions (communication breakdowns between nodes).

### Polyglot Persistence
Using different data storage technologies for different microservices based on their specific requirements.

### Portability
The ability of software to be easily moved between different environments or platforms.

### Producer-Consumer
Messaging pattern where producers create messages and consumers process them, enabling decoupled communication.

## Q

### Query-Side Caching
Caching strategy specifically for read operations to improve performance and reduce database load.

### Quorum
Minimum number of nodes that must agree on a value for a distributed system to reach consensus.

## R

### Rate Limiting
Technique to control the rate of incoming or outgoing traffic to protect services from being overwhelmed.

### Reactive Programming
Programming paradigm focused on data streams and the propagation of change, enabling asynchronous, non-blocking applications.

### Registry
Service registry that maintains information about available service instances for service discovery.

### Resilience
The ability of a system to recover from failures and continue operating correctly.

### REST (Representational State Transfer)
Architectural style for distributed systems, using HTTP protocols and standard methods (GET, POST, PUT, DELETE) for communication.

### Retry Pattern
Pattern where failed operations are automatically retried with configurable backoff strategies.

## S

### Saga Pattern
Pattern for managing distributed transactions by breaking them into a sequence of local transactions, each with compensating transactions for rollback.

### Scalability
The ability of a system to handle growing amounts of work by adding resources (horizontal or vertical scaling).

### Server-Side Discovery
Service discovery pattern where clients make requests to a load balancer or router that determines the appropriate service instance.

### Service Discovery
Mechanism for services to find and communicate with each other without hard-coded locations.

### Service Mesh
Infrastructure layer that handles service-to-service communication, providing features like load balancing, service discovery, and security.

### Service Registry
Database of available service instances used for service discovery.

### Sidecar Pattern
Pattern where a companion application runs alongside the main service to provide additional functionality like logging, monitoring, or configuration.

### Spring Cloud
Set of tools for building common patterns in distributed systems, built on top of Spring Boot.

### Spring Cloud Config
Server for centralized configuration management in distributed systems.

### Spring Cloud Gateway
API gateway built on Spring Framework providing routing, filtering, and other cross-cutting concerns.

### Spring Cloud Stream
Framework for building message-driven microservices with a unified programming model across different message brokers.

### Spring Boot
Framework for creating stand-alone, production-grade Spring applications with minimal configuration.

### Stateful Service
Service that maintains state between requests, requiring careful consideration for scaling and reliability.

### Stateless Service
Service that doesn't maintain state between requests, making it easier to scale and more resilient to failures.

### Swagger
Open-source toolset for designing, building, and documenting RESTful APIs.

## T

### Throughput
The amount of work a system can handle in a given time period, typically measured in requests per second.

### Time-to-Live (TTL)
Expiration time for cached data or messages, after which they are considered stale and should be refreshed or discarded.

### Trace
Representation of a request's path through a distributed system, used for debugging and performance analysis.

### Transaction
Logical unit of work that must either complete entirely or not at all, ensuring data consistency.

### Twelve-Factor App
Methodology for building software-as-a-service applications that emphasizes declarative configuration, statelessness, and portability.

## U

### Ubiquitous Language
Common language used by both developers and domain experts in Domain-Driven Design to ensure clear communication.

### Upstream Service
Service that provides data or functionality to another service (the downstream service).

## V

### Vertical Scaling
Adding more resources (CPU, memory) to a single instance to handle increased load, as opposed to horizontal scaling (adding more instances).

### Versioning
Practice of maintaining multiple versions of an API or service to ensure backward compatibility during updates.

### Virtualization
Technology that allows running multiple virtual machines on a single physical machine, each with its own operating system.

## W

### WebFlux
Spring's reactive web framework, built on Project Reactor, for building non-blocking, asynchronous applications.

### WebSocket
Communication protocol that provides full-duplex communication channels over a single TCP connection, useful for real-time applications.

## Z

### Zero Downtime Deployment
Deployment strategy where applications are updated without any interruption in service availability.

### Zipkin
Distributed tracing system that helps gather timing data for troubleshooting latency issues in microservices architectures.

---

This glossary covers the most common terms you'll encounter in microservices interviews and documentation. Understanding these terms is essential for effective communication about distributed systems architecture.
