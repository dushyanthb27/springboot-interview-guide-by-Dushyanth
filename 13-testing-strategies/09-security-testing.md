# How do you test security configurations? 🟠

## Question

How do you test Spring Security configurations in Spring Boot applications?

## Answer

Security configuration testing involves testing authentication, authorization, and security rules. Spring Security Test provides @WithMockUser for testing with mock users, @WithUserDetails for custom user details, and SecurityMockMvcRequestPostProcessors for testing secured endpoints.

Implementation involves using @WithMockUser to simulate authenticated users, testing secured endpoints with proper credentials, and verifying that unauthorized access is properly rejected.

## Code Example

```java
@SpringBootTest
@AutoConfigureMockMvc
class OrderControllerSecurityTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    @WithMockUser(roles = "ADMIN")
    void testDeleteOrderAsAdmin() throws Exception {
        mockMvc.perform(delete("/api/orders/1"))
                .andExpect(status().isOk());
    }

    @Test
    @WithMockUser(roles = "USER")
    void testDeleteOrderAsUser() throws Exception {
        mockMvc.perform(delete("/api/orders/1"))
                .andExpect(status().isForbidden());
    }

    @Test
    void testDeleteOrderUnauthenticated() throws Exception {
        mockMvc.perform(delete("/api/orders/1"))
                .andExpect(status().isUnauthorized());
    }
}
```

## 🎯 Interviewer Insight

The interviewer wants to test your understanding of security testing and your ability to verify security configurations.

## ⚠️ Common Mistake
Not testing unauthorized access scenarios, leaving security vulnerabilities undetected.

## ⚡ Follow-up Questions

1. How do you test custom authentication?
2. What is the difference between @WithMockUser and @WithUserDetails?
3. How do you test OAuth2 security?

## 📚 Related Topics

- [09. Security with OAuth2](../09-security-oauth2/README.md)
- [Integration Testing](./03-integration-testing.md)
- [REST API Testing](./04-rest-api-testing.md)

## Navigation

[← Previous Question](./08-test-slices.md) | [Next Question →](./10-contract-testing.md)
