# Live Coding Strategies

Tips and strategies for handling live coding interviews in Spring Boot microservices.

## Preparation

### Set Up Your Environment

**Before the Interview**:
- Have your IDE ready (IntelliJ IDEA recommended)
- Ensure JDK 17+ is installed and configured
- Have Spring Initializr bookmarked
- Practice with sample projects
- Set up common dependencies in your local Maven/Gradle cache

**Recommended Tools**:
- IntelliJ IDEA (Community or Ultimate)
- Maven or Gradle
- Git (for version control)
- Postman or similar API testing tool
- Docker (if containerization is discussed)

### Know Your Shortcuts

**Essential IntelliJ Shortcuts**:
- `Ctrl+Space`: Code completion
- `Ctrl+Shift+Enter`: Complete statement
- `Alt+Enter`: Quick fix
- `Ctrl+O`: Override methods
- `Ctrl+I`: Implement methods
- `Ctrl+Alt+L`: Reformat code
- `Ctrl+Shift+F`: Global search
- `Ctrl+Shift+R`: Global replace

## Common Live Coding Scenarios

### Scenario 1: Building a Simple REST API

**Task**: Create a REST API for managing users with CRUD operations.

**Approach**:
1. **Start with the entity**: Define the User class with JPA annotations
2. **Create the repository**: Extend JpaRepository
3. **Build the service**: Add business logic
4. **Create the controller**: Add REST endpoints
5. **Add validation**: Use Bean Validation annotations
6. **Test the endpoints**: Use Postman or curl

**Example Time Allocation**:
- Entity: 2-3 minutes
- Repository: 1 minute
- Service: 3-4 minutes
- Controller: 3-4 minutes
- Testing: 2-3 minutes

### Scenario 2: Implementing Service Communication

**Task**: Implement communication between two microservices using WebClient.

**Approach**:
1. **Create the client service**: Use WebClient for HTTP calls
2. **Add error handling**: Implement proper exception handling
3. **Add circuit breaker**: Use Resilience4j for resilience
4. **Add logging**: Log requests and responses
5. **Test the integration**: Verify end-to-end flow

**Key Points to Demonstrate**:
- Reactive programming with WebClient
- Error handling and retries
- Circuit breaker pattern
- Logging and monitoring

### Scenario 3: Adding Security

**Task**: Add Spring Security with JWT authentication to an existing API.

**Approach**:
1. **Add dependencies**: Spring Security, JWT library
2. **Create security config**: Configure security filter chain
3. **Implement JWT utilities**: Token generation and validation
4. **Add authentication filter**: JWT authentication filter
5. **Secure endpoints**: Configure authorization rules
6. **Test authentication**: Verify protected endpoints

**Key Points to Demonstrate**:
- Security configuration
- JWT token handling
- Filter implementation
- Endpoint protection

### Scenario 4: Database Operations

**Task**: Implement complex database queries with JPA.

**Approach**:
1. **Define entities**: Create proper entity relationships
2. **Create repository methods**: Use derived queries or @Query
3. **Add custom queries**: Implement complex queries with JPQL
4. **Add pagination**: Implement pageable queries
5. **Optimize performance**: Use JOIN FETCH to avoid N+1
6. **Test queries**: Verify query results

**Key Points to Demonstrate**:
- JPA entity relationships
- Custom query methods
- Pagination and sorting
- Performance optimization

### Scenario 5: Exception Handling

**Task**: Implement global exception handling for the application.

**Approach**:
1. **Create custom exceptions**: Define business exceptions
2. **Create exception responses**: Define error response structure
3. **Implement global handler**: Use @ControllerAdvice
4. **Add specific handlers**: Handle different exception types
5. **Add logging**: Log exceptions appropriately
6. **Test exception handling**: Verify error responses

**Key Points to Demonstrate**:
- Custom exception creation
- Global exception handling
- Proper error responses
- Logging and monitoring

## Live Coding Best Practices

### During the Session

**Communication**:
- Think out loud: Explain your thought process
- Ask clarifying questions: Ensure you understand requirements
- Discuss trade-offs: Explain why you chose certain approaches
- Accept feedback: Be open to suggestions and improvements

**Coding Style**:
- Write clean, readable code
- Use meaningful variable and method names
- Add comments for complex logic
- Follow Spring Boot conventions
- Structure code logically

**Time Management**:
- Start with a simple solution
- Add complexity incrementally
- Don't get stuck on details
- Focus on completing the core functionality
- Leave time for testing and refinement

### Code Organization

**Package Structure**:
```
com.example.demo
├── controller
├── service
├── repository
├── model
│   ├── entity
│   ├── dto
│   └── exception
└── config
```

**Class Structure**:
1. Class-level annotations
2. Dependencies (fields/constructor)
3. Public methods
4. Private helper methods
5. Inner classes if needed

## Common Patterns to Know

### 1. Standard Controller Pattern

```java
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
public class UserController {
    private final UserService userService;

    @GetMapping
    public ResponseEntity<List<UserDTO>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsers());
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }

    @PostMapping
    public ResponseEntity<UserDTO> createUser(@Valid @RequestBody UserDTO userDTO) {
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(userService.createUser(userDTO));
    }
}
```

### 2. Service Layer Pattern

```java
@Service
@RequiredArgsConstructor
@Transactional
public class UserService {
    private final UserRepository userRepository;
    private final UserMapper userMapper;

    public UserDTO createUser(UserDTO userDTO) {
        User user = userMapper.toEntity(userDTO);
        User savedUser = userRepository.save(user);
        return userMapper.toDTO(savedUser);
    }
}
```

### 3. Repository Pattern

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
}
```

### 4. Exception Handling Pattern

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        ErrorResponse error = new ErrorResponse("USER_NOT_FOUND", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
}
```

## Testing Your Code

### Unit Testing

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    void createUser_shouldReturnUser() {
        User user = new User("John", "john@example.com");
        when(userRepository.save(any())).thenReturn(user);

        User result = userService.createUser(user);

        assertNotNull(result);
        assertEquals("John", result.getName());
    }
}
```

### Integration Testing

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerIntegrationTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    void createUser_shouldReturnCreated() throws Exception {
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"John\",\"email\":\"john@example.com\"}"))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.name").value("John"));
    }
}
```

## Handling Pressure

### If You Get Stuck

1. **Take a breath**: Don't panic, take a moment to think
2. **Ask for clarification**: Maybe you misunderstood the requirement
3. **Break it down**: Divide the problem into smaller parts
4. **Skip and come back**: Move to another part and return later
5. **Talk through it**: Explain your thought process to the interviewer

### If You Make a Mistake

1. **Acknowledge it**: Admit the mistake calmly
2. **Fix it**: Correct the mistake promptly
3. **Learn from it**: Explain what you learned
4. **Move on**: Don't dwell on it, continue with the task

## Time Management Strategies

### Prioritization

1. **Core functionality first**: Implement the main requirements
2. **Error handling second**: Add proper exception handling
3. **Optimization third**: Optimize only if time permits
4. **Testing fourth**: Add basic tests if time allows

### Quick Wins

- Use Spring Boot starters for quick setup
- Use Lombok to reduce boilerplate
- Use derived query methods in repositories
- Use @Valid for automatic validation
- Use @ControllerAdvice for global exception handling

## Post-Session Reflection

After the live coding session, reflect on:

1. **What went well**: Identify your strengths
2. **What could be improved**: Areas to work on
3. **What you learned**: New techniques or patterns
4. **What to practice**: Skills that need more work

## Practice Resources

### Platforms for Practice

- **LeetCode**: Algorithm and data structure practice
- **HackerRank**: Coding challenges and interviews
- **Codewars**: Gamified coding challenges
- **Spring Initializr**: Quick project setup
- **Spring Boot Samples**: Official Spring Boot examples

### Practice Projects

1. **Todo API**: Simple CRUD operations
2. **E-commerce API**: Multiple entities and relationships
3. **User Management**: Authentication and authorization
4. **Order Processing**: Service communication
5. **Blog Platform**: Complex relationships and queries

## Final Tips

1. **Practice regularly**: Consistent practice is key
2. **Learn from mistakes**: Review and improve
3. **Stay calm**: Pressure is part of the process
4. **Communicate**: Your thought process matters
5. **Be confident**: You know more than you think

---

Live coding interviews test your ability to write clean, functional code under pressure. Practice these strategies and scenarios to build confidence and improve your performance during actual interviews.
