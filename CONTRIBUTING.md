# Contributing to Spring Boot Microservices Interview Guide

Thank you for your interest in contributing to this interview guide! We welcome contributions that help make this guide more comprehensive and valuable for the community.

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [Contribution Guidelines](#contribution-guidelines)
- [Question Template](#question-template)
- [Section README Template](#section-readme-template)
- [Code Standards](#code-standards)
- [Review Process](#review-process)

## 🚀 Getting Started

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes following the templates and guidelines
4. Commit your changes with clear messages
5. Push to the branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

## 📝 Contribution Guidelines

### Adding New Questions

When adding new questions, please ensure:

1. **Use the exact template format** - See [Question Template](#question-template) below
2. **Choose appropriate difficulty** - Use the difficulty legend from the main README
3. **Write scenario-based questions** - Frame the prompt around a realistic engineering problem, team trade-off, or production incident instead of a pure definition question
4. **Assess decision-making, not memorization** - Ask candidates to explain trade-offs, failure modes, and operational impact
5. **Provide working code examples** - All code must be compilable and follow Spring Boot 3.x
6. **Include all required sections** - Question, answer, code, insights, mistakes, follow-ups, related topics
7. **Use modern Java syntax** - Java 17+ features where appropriate
8. **Follow naming conventions** - Files should be named `01-topic-name.md`, `02-topic-name.md`, etc.

### Adding New Sections

Before adding a new section:

1. Check if the topic fits within existing sections
2. Ensure the topic is relevant to Spring Boot microservices
3. Follow the [Section README Template](#section-readme-template)
4. Include 10-19 question files
5. Update the main README.md with the new section

### Improving Existing Content

When improving existing content:

1. Preserve the structure and formatting
2. Update code examples to use latest Spring Boot 3.x syntax
3. Add more comprehensive explanations if needed
4. Fix any bugs or inaccuracies in code examples
5. Ensure all links are working

## 📄 Question Template

Use this exact template for all question files:

```markdown
# Question Title 🟢/🟡/🟠/🔴

## Question

[Full question text here]

## Answer

[Comprehensive answer based on difficulty level:
- 🟢: 4-6 lines
- 🟡: 6-10 lines
- 🟠: 10-15 lines
- 🔴: 15-25 lines]

## Code Example

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

// Complete, working example with imports
// Use realistic class and method names
// Add comments for non-obvious patterns
```

## 🎯 Interviewer Insight

[What the interviewer is looking for - key points they want to hear]

## ⚠️ Common Mistake

[Required for 🟠 and 🔴 questions - common pitfalls candidates make]

## ⚡ Follow-up Questions

1. [Related follow-up question 1]
2. [Related follow-up question 2]
3. [Related follow-up question 3]

## 📚 Related Topics

- [Link to related section](../section-name/README.md)
- [Link to specific question](../section-name/XX-question-name.md)

## Navigation

[← Previous Question](../section-name/XX-previous-question.md) | [Next Question →](../section-name/XX-next-question.md)
```

### Template Guidelines

- **Difficulty Badge**: Choose one: 🟢 (Beginner), 🟡 (Intermediate), 🟠 (Advanced), 🔴 (Expert)
- **Scenario-first wording**: Questions should reflect real product, team, or production problems, not just textbook definitions
- **Trade-off analysis**: Encourage candidates to explain why they would choose a pattern, when it breaks down, and what they would monitor or evolve
- **Answer Length**: Strictly follow the length guidelines based on difficulty
- **Code Examples**: Must be complete, compilable Java code with proper imports
- **Common Mistake**: Required for 🟠 and 🔴 questions, optional for 🟢 and 🟡
- **Follow-up Questions**: 2-4 relevant follow-up questions
- **Related Topics**: Link to 2-4 related sections or questions

## 📁 Section README Template

Use this template for section README files:

```markdown
# Section Name

[Section overview and description]

## 📋 Prerequisites

- [Prerequisite 1]
- [Prerequisite 2]
- [Prerequisite 3]

## 📚 Question Index

| # | Question | Difficulty |
|---|----------|------------|
| 01 | [Question Title](./01-question-name.md) | 🟢/🟡/🟠/🔴 |
| 02 | [Question Title](./02-question-name.md) | 🟢/🟡/🟠/🔴 |
| ... | ... | ... |

## 🎯 Key Takeaways

- [Key takeaway 1]
- [Key takeaway 2]
- [Key takeaway 3]

## 🔗 Related Sections

- [Related Section 1](../section-name/README.md)
- [Related Section 2](../section-name/README.md)

## 📖 Resources

- [Official Documentation Link](url)
- [Additional Resource](url)
```

## 💻 Code Standards

### Java Code Requirements

1. **Java Version**: Use Java 17+ syntax
2. **Spring Boot Version**: Use Spring Boot 3.x annotations and configurations
3. **Configuration**: Use Java-based configuration (@Configuration classes), no XML
4. **Package Structure**: Include proper package declarations
5. **Imports**: Include all necessary imports
6. **Comments**: Add comments explaining non-obvious patterns
7. **Naming**: Use realistic, production-like class and method names

### Example Structure

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class RestClientConfig {

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

### Code Quality

- Code must be compilable
- Follow Java naming conventions
- Use meaningful variable names
- Include proper exception handling where relevant
- Demonstrate best practices appropriate to the difficulty level

## 🔍 Review Process

### Before Submitting

1. **Check formatting** - Ensure markdown is properly formatted
2. **Test code examples** - Verify code is syntactically correct
3. **Check links** - Ensure all internal and external links work
4. **Verify templates** - Use the exact templates provided
5. **Spell check** - Check for typos and grammatical errors

### Pull Request Process

1. Submit a PR with a clear title and description
2. Reference any related issues
3. Wait for review - maintainers will review within 3-5 business days
4. Address review feedback promptly
5. Once approved, the PR will be merged

### Review Criteria

- Adherence to templates and formatting
- Code quality and correctness
- Accuracy of technical content
- Relevance to Spring Boot microservices
- Clarity and completeness of explanations
- Proper difficulty classification

## 🎯 Types of Contributions We Welcome

- ✅ New interview questions following the template
- ✅ Improvements to existing questions
- ✅ Additional code examples
- ✅ Bug fixes in code examples
- ✅ Documentation improvements
- ✅ New cheatsheets
- ✅ Additional resources
- ✅ Translation to other languages
- ✅ System design questions
- ✅ Behavioral interview scenarios

## ❌ What We Don't Accept

- ❌ Questions not related to Spring Boot microservices
- ❌ Outdated Spring Boot 2.x examples (unless specifically noted)
- ❌ XML-based configurations
- ❌ Incomplete or non-compilable code
- ❌ Questions without proper formatting
- ❌ Plagiarized content
- ❌ Offensive or inappropriate content

## 📧 Contact

For questions about contributing, please open an issue or contact the maintainers.

## 📜 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to the Spring Boot Microservices Interview Guide! 🚀
