# How do you implement multi-tenancy? 🟠

## Question

How do you implement multi-tenancy in Spring Boot applications?

## Answer

Multi-tenancy allows a single application to serve multiple tenants (customers/organizations) with data isolation. Approaches include database per tenant (separate database for each tenant), schema per tenant (separate schema in same database), and shared schema with tenant ID column. Each approach has different trade-offs in isolation, cost, and complexity.

Implementation involves configuring tenant resolution (from request header, subdomain, or JWT), implementing tenant-aware datasource routing, and ensuring data is properly isolated per tenant.

## Code Example

```java
@Configuration
public class MultiTenancyConfig {
    @Bean
    public DataSource multiTenantDataSource() {
        Map<Object, Object> targetDataSources = new HashMap<>();
        targetDataSources.put("tenant1", tenant1DataSource());
        targetDataSources.put("tenant2", tenant2DataSource());

        MultiTenantDataSource routingDataSource = new MultiTenantDataSource();
        routingDataSource.setTargetDataSources(targetDataSources);
        routingDataSource.setDefaultTargetDataSource(tenant1DataSource());
        return routingDataSource;
    }
}

@Component
public class TenantInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String tenantId = request.getHeader("X-Tenant-ID");
        TenantContext.setTenantId(tenantId);
        return true;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of multi-tenancy patterns and your ability to implement tenant isolation.

## ⚠️ Common Mistake
Not properly isolating tenant data, leading to data leakage between tenants.

## ⚡ Follow-up Questions

1. What are the different multi-tenancy approaches?
2. How do you choose the right approach for your use case?
3. How do you handle tenant-specific configurations?

## 📚 Related Topics

- [Database per Service](./01-database-per-service.md)
- [Database Security](./14-database-security.md)
- [Database Best Practices](./13-database-best-practices.md)

## Navigation

[← Previous Question](./10-caching-strategies.md) | [Next Question →](./12-distributed-transactions.md)
