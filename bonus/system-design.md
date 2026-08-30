# System Design Questions

Advanced system design questions for senior and principal level interviews.

## E-commerce System Design

### Question: Design a scalable e-commerce platform

**Key Components to Discuss:**
- Microservices decomposition (User, Product, Order, Payment, Inventory)
- Database per service pattern
- Event-driven architecture for order processing
- API gateway for routing and rate limiting
- Caching strategy for product catalog
- Search service with Elasticsearch
- Payment integration with external providers
- Inventory management with eventual consistency

**Architecture Considerations:**
- Service boundaries based on business domains
- Data consistency patterns (Saga for distributed transactions)
- High availability and fault tolerance
- Scalability strategies (horizontal scaling, caching)
- Security (OAuth2, JWT, PCI compliance)

## Social Media Platform

### Question: Design a social media feed system

**Key Components to Discuss:**
- User service with profile management
- Post service with content storage
- Feed service with timeline generation
- Notification service for real-time updates
- Relationship service for connections
- Media service for image/video storage
- Analytics service for engagement tracking

**Architecture Considerations:**
- Fan-out pattern for feed generation
- Event sourcing for post history
- Caching strategies (Redis for hot feeds)
- Database sharding for user data
- CDN for media delivery
- Real-time updates with WebSockets

## Real-Time Chat System

### Question: Design a real-time messaging application

**Key Components to Discuss:**
- User authentication and presence
- Message service with WebSocket support
- Conversation management
- Message persistence and history
- Push notification service
- Media sharing service
- Encryption and security

**Architecture Considerations:**
- WebSocket vs long-polling
- Message ordering and delivery guarantees
- Offline message handling
- End-to-end encryption
- Scalability of WebSocket connections
- Database design for message storage

## Payment Processing System

### Question: Design a secure payment processing system

**Key Components to Discuss:**
- Payment gateway integration
- Transaction service
- Fraud detection service
- Reconciliation service
- Notification service
- Audit logging
- Compliance monitoring

**Architecture Considerations:**
- PCI DSS compliance
- Idempotency for payment operations
- Circuit breakers for external APIs
- Distributed transactions with Saga
- Real-time fraud detection
- Audit trail and monitoring

## Food Delivery System

### Question: Design a food delivery platform

**Key Components to Discuss:**
- User and restaurant management
- Menu and ordering service
- Order processing workflow
- Driver dispatch and tracking
- Real-time location updates
- Payment processing
- Rating and review system

**Architecture Considerations:**
- Geospatial queries for restaurant/driver matching
- Real-time updates with WebSockets
- Order state machine
- Driver allocation algorithms
- ETA calculation
- Multi-region deployment

## Streaming Service

### Question: Design a video streaming platform

**Key Components to Discuss:**
- Content management service
- Video encoding and transcoding
- CDN integration
- User authentication and authorization
- Recommendation engine
- Playback service
- Analytics and monitoring

**Architecture Considerations:**
- Adaptive bitrate streaming
- Content delivery optimization
- DRM and content protection
- Scalability of video storage
- Recommendation algorithms
- Bandwidth optimization

## IoT Platform

### Question: Design an IoT data collection and processing platform

**Key Components to Discuss:**
- Device registration and management
- Data ingestion service
- Message broker for high-throughput data
- Data processing pipeline
- Storage service (time-series database)
- Alerting and notification service
- Device control service

**Architecture Considerations:**
- Protocol support (MQTT, CoAP, HTTP)
- High-throughput message handling
- Real-time data processing
- Data retention policies
- Device authentication and security
- Edge computing integration

## Healthcare System

### Question: Design a healthcare management system

**Key Components to Discuss:**
- Patient management service
- Electronic health records (EHR)
- Appointment scheduling
- Prescription management
- Lab results integration
- Billing and insurance
- Telemedicine service

**Architecture Considerations:**
- HIPAA compliance
- Data privacy and security
- Audit logging
- Data interoperability (FHIR)
- High availability for critical systems
- Disaster recovery

## Banking System

### Question: Design a core banking system

**Key Components to Discuss:**
- Account management
- Transaction processing
- Ledger service
- Fraud detection
- Compliance and reporting
- Customer service
- Mobile banking API

**Architecture Considerations:**
- ACID transactions
- Regulatory compliance
- Real-time fraud detection
- High availability and disaster recovery
- Data consistency across services
- Audit trails and logging

## Logistics and Supply Chain

### Question: Design a logistics and supply chain management system

**Key Components to Discuss:**
- Warehouse management
- Inventory tracking
- Shipment tracking
- Route optimization
- Carrier integration
- Supply chain visibility
- Analytics and reporting

**Architecture Considerations:**
- Real-time tracking
- Geospatial data handling
- Route optimization algorithms
- Integration with external carriers
- Event-driven architecture for updates
- Scalability for high-volume operations

## General System Design Principles

### Key Concepts to Master

**Scalability:**
- Horizontal vs vertical scaling
- Load balancing strategies
- Database sharding
- Caching at multiple levels
- CDN integration

**Reliability:**
- Redundancy and failover
- Circuit breakers and retries
- Health checks and monitoring
- Disaster recovery
- Data backup strategies

**Performance:**
- Latency optimization
- Throughput considerations
- Database indexing
- Query optimization
- Caching strategies

**Security:**
- Authentication and authorization
- Data encryption
- Network security
- Compliance requirements
- Security monitoring

**Maintainability:**
- Microservices decomposition
- API design principles
- Documentation
- Monitoring and observability
- Deployment automation

## System Design Interview Tips

1. **Clarify Requirements**: Ask about scale, constraints, and specific features
2. **High-Level Design**: Start with major components and their interactions
3. **Deep Dive**: Focus on 2-3 critical components in detail
4. **Trade-offs**: Discuss alternatives and their pros/cons
5. **Bottlenecks**: Identify potential bottlenecks and how to address them
6. **Scalability**: Explain how the system scales with increased load
7. **Data Flow**: Trace a request through the system end-to-end
8. **Failure Scenarios**: Discuss how the system handles failures

## Practice Approach

1. **Start Simple**: Begin with basic requirements
2. **Iterate**: Add complexity incrementally
3. **Justify Decisions**: Explain why you chose specific approaches
4. **Consider Alternatives**: Discuss different solutions
5. **Think About Edge Cases**: Consider unusual scenarios
6. **Draw Diagrams**: Visualize the architecture
7. **Estimate Capacity**: Calculate resource requirements
8. **Discuss Operations**: Consider deployment and monitoring

---

These system design questions test your ability to architect complex, distributed systems. Practice breaking down problems, making trade-offs, and justifying your architectural decisions.
