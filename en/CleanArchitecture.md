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
 
### Testing Domain layer in Clean Architecture
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

# Data Layer
- The Data Layer is responsible for retrieving and storing data, acting as a bridge between the data sources (local and remote) and the core business logic in the Domain Layer.
- This layer ensures that the Domain Layer remains isolated from the complexities of data handling.
- Pure Kotlin!
### Responsibilities of the Data Layer
**1. Data Provisioning**
- Supplies data required by the Domain Layer, ensuring it is properly formatted and ready for use.
**2. Separation of Concerns**
- Focuses solely on managing data operations (fetching, storing, updating, and deleting), without handling business rules or application logic.
**3. Repository as a Connector**
- Implements the Repository pattern to abstract data sources, allowing the Domain Layer to interact with a unified interface rather than dealing with specific data sources directly.
### Repository Implementation
- Repositories in the Data Layer are designed to collect data from various sources and provide it to the Domain Layer in a consistent manner.
- **Responsibility**: Implements the repository interfaces defined in the Domain Layer.
- **Data Source Integration**: Combines data from multiple sources, such as local databases and remote APIs.
### Data Sources
- The **DataSource Interface** standardizes the way data is retrieved from different sources.
- **LocalDataSource**: Handles data stored locally, such as in an SQLite database or shared preferences.
- **RemoteDataSource**: Manages data fetched from external sources, like REST APIs or GraphQL endpoints.
### Model Mapper
- Since data models in the Data Layer may differ from those in the Domain Layer, a Model Mapper is used to convert between the two.
- Converts data entities from the data source into domain models for use in the Domain Layer.
- Handles conversion in both directions (Data Layer models ↔ Domain models) to maintain separation between layers.
### Implementation Considerations
**1. Data Fetching Strategy**
- When implementing the Data Layer, it's important to define a clear strategy for fetching data
- **Remote-Only Fetching**: Retrieve data directly from the remote source without using local data.
- **Local-First Approach**: Display locally cached data first and then update it by fetching from the remote source.
> **Potential Challenges**  
> - **Data Mismatch**: How will the app handle situations where local and remote data differ?
> - **Structural Differences**: How will differing data structures between local and remote sources be reconciled?
**2. Error Handling**
- Proper error handling is critical when interacting with remote data sources.
- Implement strategies to handle common errors such as network issues, server unavailability, or timeouts.
- Provide fallback mechanisms, such as displaying cached data when remote data cannot be fetched.
> **Key Points to Know**
> - Repository Design: Ensure that each repository is responsible for only one specific task.
> - Reactive Programming: Handle asynchronous data streams using Flow or RxJava.
> - State Management: Design to handle and manage states such as Loading, Success, and Error.

# Remote Layer
- Responsible for communicating with remote servers to send and receive data.
- Includes handling of data requests, responses, and network-related errors.
- Manages API calls using networking libraries such as Retrofit (with OkHttp).
- Retrofit can be used not only on the Android platform but also as pure Kotlin code.
- Theoretically, it can be used without depending on the Android platform, but in real-world project implementations, this is often not the case.

# Local Layer
- A layer responsible for storing and reading local data.
- Local data is managed through DB, SharedPreferences, and the file system.
- Local data should be designed with the assumption that "it can be deleted at any time"
  - Most of the returned data should be nullable to account for this possibility.
 
# Presentation Layer
- **ViewModel**: Manages UI data and handles user actions.
- **State Management**: Manages the UI state using LiveData or StateFlow.
- **Business Logic Invocation**: Calls UseCases to either fetch or send data.
- **Error Handling and UI Notifications**: Passes error or event notifications to the UI layer.

> ### Android ViewModel?
> - **Theoretical Concept**: ViewModel should be independent of Android’s ViewModel implementation.
> - **In Practice**: It is often unavoidable to use Android’s ViewModel in real implementations.
> - **Guideline**: The ViewModel should not include Android-specific packages.
>   - Avoid importing android.* packages.
>   - Exception: AAC-related code (e.g., android.arch.*) is allowed due to the reasons mentioned above.

> ### Point
> - One-Way Data Flow
>   - Data flows from the ViewModel to the UI.
>   - Events (Actions) are sent from the UI to the ViewModel.
> - State Preservation
>   - Ensure that the data state is retained even when the screen is redrawn due to events like screen rotation.

# UI Layer
- Displays data on the screen and handles user input to trigger business logic.
- Does not directly access business logic; instead, it interacts with the Presentation Layer.
  - Ignores the Domain Layer.
  - Uses only the data provided by the ViewModel and sends actions.
