# What is the difference between singleton and prototype scope? 🟠

## Question

Explain the different bean scopes in Spring Boot and when to use singleton vs prototype scope.

## Answer

Spring Boot supports several bean scopes: singleton (default, one instance per Spring container), prototype (new instance each time requested), request (one instance per HTTP request), session (one instance per HTTP session), application (one instance per ServletContext), and websocket (one instance per WebSocket). Singleton scope is the default and most common, suitable for stateless services. Prototype scope is used when you need a new instance each time, such as for stateful objects or command objects.

The choice of scope impacts performance, memory usage, and thread safety. Singleton beans must be thread-safe if they maintain state, while prototype beans avoid state sharing issues but may have higher memory overhead. Request and session scopes are specific to web applications and require proper proxy configuration for injection into singleton beans.

## Code Example

```java
package com.example.demo.service;

import org.springframework.context.annotation.Scope;
import org.springframework.context.annotation.ScopedProxyMode;
import org.springframework.stereotype.Component;
import org.springframework.web.context.WebApplicationContext;

// Singleton scope (default)
@Component
public class SingletonService {
    private int counter = 0;

    public int incrementCounter() {
        return ++counter; // Not thread-safe!
    }
}

// Thread-safe singleton service
@Component
public class ThreadSafeSingletonService {
    private final AtomicInteger counter = new AtomicInteger(0);

    public int incrementCounter() {
        return counter.incrementAndGet();
    }
}

// Prototype scope
@Component
@Scope("prototype")
public class PrototypeService {
    private int counter = 0;

    public int incrementCounter() {
        return ++counter;
    }
}

// Request scope (web application)
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestScopedService {
    private String requestId;

    public void setRequestId(String requestId) {
        this.requestId = requestId;
    }

    public String getRequestId() {
        return requestId;
    }
}

// Session scope (web application)
@Component
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class SessionScopedService {
    private String userId;

    public void setUserId(String userId) {
        this.userId = userId;
    }

    public String getUserId() {
        return userId;
    }
}

// Using prototype bean in singleton
@Service
public class OrderService {
    private final ApplicationContext applicationContext;

    public OrderService(ApplicationContext applicationContext) {
        this.applicationContext = applicationContext;
    }

    public Order createOrder() {
        // Get new prototype instance each time
        Order order = applicationContext.getBean(Order.class);
        order.setCreatedAt(LocalDateTime.now());
        return order;
    }
}

// Custom scope configuration
@Configuration
public class CustomScopeConfig implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
        beanFactory.registerScope("thread", new SimpleThreadScope());
    }
}

@Component
@Scope("thread")
public class ThreadScopedService {
    private String threadData;

    public void setThreadData(String threadData) {
        this.threadData = threadData;
    }

    public String getThreadData() {
        return threadData;
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of bean scopes and your ability to choose the appropriate scope for different use cases. They're looking for awareness of thread safety implications and the differences between singleton and prototype patterns.

## ⚠️ Common Mistake

Using singleton scope for stateful objects without proper synchronization, leading to thread safety issues. Another mistake is not understanding that injecting a prototype bean into a singleton bean won't create new instances automatically.

## ⚡ Follow-up Questions

1. How do you inject a prototype bean into a singleton bean?
2. What are the web-specific bean scopes and when would you use them?
3. How do you create custom bean scopes?

## 📚 Related Topics

- [Dependency Injection](./04-dependency-injection.md)
- [Bean Configuration](./13-bean-configuration.md)
- [Application Lifecycle](./08-application-lifecycle.md)

## Navigation

[← Previous Question](./13-bean-configuration.md) | [Next Question →](./15-conditional-beans.md)
