# How do you customize the JSON response in Spring Data REST? 🟠

## Question

What are the different ways to customize JSON serialization and deserialization in Spring Data REST?

## Answer

JSON customization in Spring Data REST can be achieved through Jackson annotations on entities, implementing custom serializers/deserializers, configuring ObjectMapper settings, using @JsonView for different response shapes, and implementing RepositoryRestConfigurer for global configuration. Spring Data REST uses Jackson for JSON processing by default.

Customization approaches include using @JsonIgnore, @JsonProperty, @JsonFormat annotations on entity fields, creating custom JsonSerializer and JsonDeserializer implementations, configuring ObjectMapper in RepositoryRestConfigurer, using @JsonView for projection-like behavior, and implementing custom type information handling.

## Code Example

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @JsonProperty("user_name")
    private String name;

    @JsonIgnore
    private String password;

    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd")
    private LocalDate birthDate;
}

// Custom serializer
public class UserSerializer extends JsonSerializer<User> {
    @Override
    public void serialize(User user, JsonGenerator gen, SerializerProvider serializers)
            throws IOException {
        gen.writeStartObject();
        gen.writeNumberField("id", user.getId());
        gen.writeStringField("name", user.getName());
        gen.writeEndObject();
    }
}

@Configuration
public class JacksonConfig implements RepositoryRestConfigurer {

    @Override
    public void configureJacksonObjectMapper(ObjectMapper objectMapper) {
        objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        objectMapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of JSON processing in Spring Data REST and your ability to customize API responses.

## ⚠️ Common Mistake
Over-customizing JSON responses to the point where they lose the benefits of standard Spring Data REST responses. Another mistake is not considering the impact on HATEOAS links when customizing serialization.

## ⚡ Follow-up Questions

1. How do you handle date formatting in JSON responses?
2. What is the difference between @JsonIgnore and @JsonView?
3. How do you implement custom deserialization for nested objects?

## 📚 Related Topics

- [Repository Projections](./03-repository-projections.md)
- [Customizing Endpoints](./04-customizing-endpoints.md)
- [HATEOAS](./05-hateoas.md)

## Navigation

[← Previous Question](./09-validation.md) | [Next Question →](./11-limitations.md)
