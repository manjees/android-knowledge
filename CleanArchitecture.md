# Clean Architecture in Android Applications
- Clean Architecture is a widely adopted software design paradigm in Android development. It emphasizes clear separation of concerns across layers, helping developers create scalable, testable, and maintainable apps.

## Key Objectives in Android Development
- **Dependency Control**: Decoupling business logic from UI and platform-specific components.
- **Resilience to Change**: Ensuring that changes in external systems (UI, APIs, databases) do not affect the core business logic.
- **Platform Independence**: Core logic should remain independent of Android-specific elements.

## Essential Considerations
**1. Lifecycle Awareness**
- Properly handle lifecycle events such as screen rotations, backgrounding, and activity/fragment destruction.
**2. Efficient Resource Management**
- Optimize app performance by managing memory and power usage
  - Prevent memory leaks by avoiding long-lived references.
  - Use appropriate strategies for handling large objects like bitmaps.
**3. Modular Structure**
- Divide the app into independent modules representing different layers. This facilitates isolated testing, parallel development, and clear responsibility boundaries.
**4. Permission Handling**
- Implement a robust permission management system to handle runtime permissions required by app features.

# Domain Layer: The Core of Clean Architecture
- The **Domain Layer** encapsulates the business rules and logic, acting as the foundation of the application. This layer is isolated from both UI and data concerns.

## Characteristics of the Domain Layer
- Pure Kotlin code with no dependency on the Android framework.
- Immune to changes in external systems (UI, API, databases).
- Contains two key components
  - **Entities**: Represent core business models.
  - **Use Cases**: Encapsulate specific business processes.

 ## Components of the Domain Layer
 **1. Entity**
 - An Entity is a plain Kotlin object representing a fundamental business concept.
   - It serves as a data holder for business rules.
   - Acts as a Domain Transfer Object (DTO) to convey information across layers.
**2. Use Case**
- A Use Case represents a specific operation or interaction within the business domain.
  - Each Use Case should focus on a single responsibility (SRP – Single Responsibility Principle).
  - It communicates with the Repository to execute its logic.
  ```kotlin
  class FetchUserDetailsUseCase(private val repository: UserRepository) {
      operator fun invoke(userId: String): UserDetails {
          return repository.getUserDetails(userId)
      }
  }
  ```
**3. Repository**
- The Repository abstracts access to data sources (local database, remote API, cache, etc.).
  - The Domain Layer interacts with repositories through interfaces only.
  - The actual data source implementation resides in the Data Layer.
  - This abstraction ensures that business logic remains decoupled from specific data source details.
 
# Testing in Clean Architecture
- One of the major advantages of Clean Architecture is its support for robust and independent unit testing.
  - **Domain Layer Testing**: Since the domain layer is free from platform dependencies, Use Cases can be tested easily in isolation.
  - **Repository Interface Mocking**: During tests, you can mock the repository interface without involving actual data sources, ensuring that only business logic is validated.
  ```kotlin
  @Test
  fun `verify user details retrieval`() {
    val mockRepository = mock(UserRepository::class.java)
    val useCase = FetchUserDetailsUseCase(mockRepository)

    val userDetails = UserDetails("John", "Doe")
    whenever(mockRepository.getUserDetails("1")).thenReturn(userDetails)

    assertEquals(userDetails, useCase("1"))
  }
  ```

> **Domain Layer as a Planner**  
> - Think of the Domain Layer as a planner who designs the overall flow of tasks.
>   - The planner defines the goals and strategy (Use Cases) without worrying about who performs the task or how it's done (data source details).
>   - Focus remains purely on what needs to be achieved, without concern for external changes (UI updates, server API changes).

> **Best Practices for the Domain Layer**  
> - Stability: Ensure the Domain Layer remains unchanged despite modifications in UI, APIs, or data sources.
> - Meaningful Naming: Use clear and descriptive names for classes, functions, and variables to improve readability.
> - Isolated Code: Keep the Domain Layer free from platform-specific dependencies. This promotes better portability and maintainability.
