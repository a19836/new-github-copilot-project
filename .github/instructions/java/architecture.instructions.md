---
applyTo: "**/*.java"
description: "Java-specific architecture add-on for the shared hexagonal architecture guidelines. This file extends .github/instructions/architecture.instructions.md and adds Spring Boot-specific implementation guidance without repeating the generic baseline."
---

# Java Spring Boot – Hexagonal Architecture Extensions

> This file extends `.github/instructions/architecture.instructions.md`.
> It adds Java and Spring Boot-specific guidance only where the language-agnostic baseline is too general.
> The shared architecture rules remain the source of truth for dependency direction, ports/adapters, and layered design.

## 1. Default Java behavior

For new Spring Boot Java projects, the default architecture remains **hexagonal architecture (ports and adapters)**.

Use this pattern:

- keep the domain and application logic independent from Spring Boot, HTTP, and persistence frameworks
- define business use cases as input ports and external dependencies as output ports
- implement HTTP, persistence, messaging, and external integrations as infrastructure adapters
- keep framework configuration and wiring in infrastructure/config

## 2. Java layer expectations

### Domain layer

- Keep domain models and business rules free from Spring Boot, JPA, Hibernate, and HTTP types.
- Prefer immutable objects and constructor-based initialization.
- Put invariants and validation rules in the domain or application layer, not in controllers or repositories.
- Keep domain objects focused on business meaning, not persistence concerns.

### Application layer

- Define input ports as use-case interfaces such as `CreateUserUseCase` or `GetOrderUseCase`.
- Define output ports for persistence, external APIs, message queues, or storage.
- Keep application services focused on orchestration and business flow.
- Use constructor injection and avoid framework-specific dependencies in the service layer.

### Infrastructure layer

- Put controllers, REST DTOs, persistence adapters, HTTP clients, security configuration, and bean wiring here.
- Translate transport and persistence contracts into domain/application contracts using mappers/adapters.
- Keep adapters thin and explicit; do not put core business logic in them.

## 3. Java package conventions

A common Java package layout is:

```text
src/main/java/com/example/app
  domain/
    model/
    service/
    exception/
    valueobject/

  application/
    port/
      in/
      out/
    service/

  infrastructure/
    adapter/
      in/
        web/
          controller/
          dto/
          mapper/
      out/
        persistence/
          entity/
          repository/
          adapter/
          mapper/
    config/
    security/
```

The exact naming may vary, but the separation of business logic from transport and persistence stays mandatory.

## 4. Spring Boot-specific patterns

### Input ports

Use interfaces that describe business capabilities, not HTTP or repository details:

```java
public interface GetUserUseCase {
    User getUser(UserId id);
}

public interface CreateUserUseCase {
    User createUser(CreateUserCommand command);
}
```

### Output ports

Use interfaces for dependencies the application needs:

```java
public interface UserRepositoryPort {
    Optional<User> findById(UserId id);
    User save(User user);
}
```

### Application services

Application services should orchestrate use cases and depend only on domain objects and ports:

```java
public class UserService implements GetUserUseCase, CreateUserUseCase {

    private final UserRepositoryPort userRepository;

    public UserService(UserRepositoryPort userRepository) {
        this.userRepository = userRepository;
    }
}
```

### Infrastructure adapters

Infrastructure code should adapt framework concepts to the application ports:

```java
@Repository
public class UserPersistenceAdapter implements UserRepositoryPort {
    private final SpringDataUserRepository repository;
}
```

## 5. Spring Security guidance for new Spring Boot projects

For new Spring Boot Java projects, use the default Spring Security conventions unless the project has a clear need to override the standard setup.

When implementing security:

- define security configuration in the infrastructure/security layer
- use standard Spring Security components and default patterns where possible
- override default classes only when a project-specific requirement justifies it
- keep authorization checks at the edge of the application, not hidden in business logic
- do not put security policy decisions inside domain objects or services unless the rule is truly domain-specific

This means the default Spring Boot security flow remains the starting point for new Java projects, while custom overrides should be narrow, intentional, and well-justified.

## 6. Bean configuration and dependency injection

Keep Spring wiring in infrastructure configuration:

```java
@Configuration
public class BeanConfiguration {

    @Bean
    public UserService userService(UserRepositoryPort repository) {
        return new UserService(repository);
    }
}
```

Rules:

- prefer constructor injection
- keep application services free from `@Service` or `@Component` annotations
- inject interfaces, not concrete infrastructure implementations
- avoid duplicate bean registration for the same use-case service

## 7. Validation, transactions, and error handling

- Validate input at the transport boundary and re-check critical invariants in the service layer when necessary.
- Keep transaction boundaries in the application/service flow and configure them in infrastructure.
- Return stable application/domain errors rather than leaking raw persistence or framework exceptions.
- Use explicit mappers and DTOs between HTTP, application, and persistence layers.

## 8. Java review checks

Before finalizing a Java change, confirm:

- domain/application layers are free from framework and persistence leakage
- controllers only handle request/response translation and validation
- repositories and adapters implement ports rather than embedding business behavior
- Spring Security follows the default Spring Boot conventions unless a custom override is justified
- dependency direction remains inward: infrastructure → application → domain

## 9. Final objective

The Java code should remain easy to change across frameworks and persistence technologies without rewriting the core business logic.

The business layer must remain independent from whether the system is exposed over REST, messaging, or another transport, and independent from the persistence technology used underneath.
