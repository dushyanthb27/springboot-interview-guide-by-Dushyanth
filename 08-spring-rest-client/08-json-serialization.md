# How do you handle JSON serialization/deserialization? 🟡

## Question

How do you configure JSON serialization and deserialization in Spring REST clients?

## Answer

JSON serialization/deserialization in Spring REST clients is handled by Jackson message converters, which can be customized for specific requirements. Configuration includes customizing ObjectMapper settings, registering custom serializers/deserializers, and handling date/time formats.

Configuration approaches include customizing ObjectMapper with specific date formats, ignoring unknown properties, and handling null values. Custom serializers can be implemented for complex object mapping requirements.

## Code Example

```java
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient() {
        ObjectMapper objectMapper = new ObjectMapper()
                .registerModule(new JavaTimeModule())
                .disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)
                .setSerializationInclusion(JsonInclude.Include.NON_NULL);

        ExchangeStrategies strategies = ExchangeStrategies.builder()
                .codecs(configurer -> {
                    configurator.defaultCodecs()
                            .maxInMemorySize(16 * 1024 * 1024)
                            .customCodecs(codecConfigurer -> codecConfigurer
                                    .defaultCodecs()
                                    .register(new Jackson2JsonEncoder(objectMapper))
                                    .register(new Jackson2JsonDecoder(objectMapper)));
                })
                .build();

        return WebClient.builder()
                .exchangeStrategies(strategies)
                .baseUrl("http://user-service")
                .build();
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of JSON processing and your ability to customize serialization/deserialization.

## ⚠️ Common Mistake
Not configuring date formats properly, leading to parsing errors or inconsistent date representations.

## ⚡ Follow-up Questions

1. How do you handle date/time serialization?
2. What is the purpose of custom serializers?
3. How do you handle null values in JSON?

## 📚 Related Topics

- [WebClient Configuration](./03-webclient-configuration.md)
- [Error Handling](./04-error-handling.md)
- [03. Spring Data REST](../03-spring-data-rest/README.md)

## Navigation

[← Previous Question](./07-connection-pooling.md) | [Next Question →](./09-request-logging.md)
