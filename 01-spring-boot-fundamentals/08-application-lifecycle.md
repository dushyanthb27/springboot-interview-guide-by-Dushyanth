# Explain the Spring Boot application lifecycle 🟠

## Question

Describe the lifecycle of a Spring Boot application from startup to shutdown, including the key phases and hooks available.

## Answer

The Spring Boot application lifecycle consists of several phases: environment preparation, context creation, bean instantiation and dependency injection, property loading, auto-configuration processing, application context refresh, and application startup. During startup, Spring Boot processes SpringApplication events like ApplicationStartingEvent, ApplicationEnvironmentPreparedEvent, ApplicationPreparedEvent, ApplicationStartedEvent, and ApplicationReadyEvent. During shutdown, it processes ContextClosedEvent and ApplicationPreparedEvent.

Key lifecycle hooks include @PostConstruct for bean initialization, @PreDestroy for bean destruction, CommandLineRunner and ApplicationRunner for startup logic, SmartLifecycle for advanced lifecycle management, and DisposableBean for cleanup operations. Understanding these phases is crucial for proper resource management and startup/shutdown logic.

## Code Example

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.ApplicationListener;
import org.springframework.context.annotation.Bean;
import org.springframework.context.event.ContextClosedEvent;
import org.springframework.context.event.EventListener;

import jakarta.annotation.PostConstruct;
import jakarta.annotation.PreDestroy;

@SpringBootApplication
public class LifecycleApplication {

    public static void main(String[] args) {
        SpringApplication.run(LifecycleApplication.class, args);
    }

    // Bean initialization hook
    @PostConstruct
    public void init() {
        System.out.println("Bean initialized");
    }

    // Bean destruction hook
    @PreDestroy
    public void cleanup() {
        System.out.println("Bean destroyed");
    }

    // Application startup hook
    @Bean
    public CommandLineRunner startupRunner() {
        return args -> {
            System.out.println("Application started with args: " + String.join(", ", args));
        };
    }

    // Application ready event listener
    @EventListener(ApplicationReadyEvent.class)
    public void onApplicationReady() {
        System.out.println("Application is ready to serve requests");
    }

    // Context closed event listener
    @EventListener(ContextClosedEvent.class)
    public void onContextClosed() {
        System.out.println("Application context is closing");
    }
}

// Custom lifecycle management
@Component
public class CustomLifecycleManager implements SmartLifecycle {

    private boolean isRunning = false;

    @Override
    public void start() {
        isRunning = true;
        System.out.println("Custom lifecycle started");
    }

    @Override
    public void stop() {
        isRunning = false;
        System.out.println("Custom lifecycle stopped");
    }

    @Override
    public boolean isRunning() {
        return isRunning;
    }

    @Override
    public boolean isAutoStartup() {
        return true;
    }

    @Override
    public void stop(Runnable callback) {
        stop();
        callback.run();
    }

    @Override
    public int getPhase() {
        return 1; // Determines startup order
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of Spring Boot's internal workings and your ability to hook into the application lifecycle. They're looking for knowledge of when different initialization and cleanup operations should occur.

## ⚠️ Common Mistake

Not understanding the order of lifecycle events, leading to operations being performed at the wrong time. Another mistake is not properly implementing SmartLifecycle interface methods, causing lifecycle management issues.

## ⚡ Follow-up Questions

1. What is the difference between CommandLineRunner and ApplicationRunner?
2. How do you control the order of bean initialization?
3. What happens during the application context refresh phase?

## 📚 Related Topics

- [Bean Configuration](./13-bean-configuration.md)
- [Bean Scopes](./14-bean-scopes.md)
- [Conditional Beans](./15-conditional-beans.md)

## Navigation

[← Previous Question](./07-dependency-injection-methods.md) | [Next Question →](./09-custom-annotations.md)
