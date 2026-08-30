# What are spans and traces in distributed tracing? 🟡

## Question

Explain the concepts of spans and traces in distributed tracing.

## Answer

A trace represents the entire journey of a request through the system, from entry point to exit. A span represents a single unit of work within a trace, such as a service call, database query, or external API call. Spans are linked together to form a trace, with parent-child relationships showing the call hierarchy.

Each span has a unique span ID, a trace ID (shared across all spans in the trace), timestamps, duration, and optional tags for additional context. The root span is the first span in the trace, and child spans represent nested operations.

## Code Example

```java
// Trace structure example
// Trace ID: abc123 (shared across all spans)
// 
// Span 1 (Root): API Gateway receives request
//   - Span ID: 1
//   - Parent ID: null
// 
// Span 2: Service A processes request
//   - Span ID: 2
//   - Parent ID: 1
// 
// Span 3: Service B called by Service A
//   - Span ID: 3
//   - Parent ID: 2
// 
// Span 4: Database query in Service B
//   - Span ID: 4
//   - Parent ID: 3
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of the core data structures in distributed tracing and how they represent request flow.

## ⚠️ Common Mistake
Not understanding the parent-child relationship between spans, leading to incorrect trace interpretation.

## ⚡ Follow-up Questions

1. What information is stored in a span?
2. How are spans linked together?
3. What is the difference between a root span and a child span?

## 📚 Related Topics

- [How Sleuth Works](./02-how-sleuth-works.md)
- [Trace Context Propagation](./05-trace-context-propagation.md)
- [Custom Tags](./06-custom-tags.md)

## Navigation

[← Previous Question](./03-sleuth-zipkin-integration.md) | [Next Question →](./05-trace-context-propagation.md)
