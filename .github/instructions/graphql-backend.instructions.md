---
applyTo:
  - "**/*fetcher*.java"
  - "**/*datafetcher*.java"
  - "**/dgs/**/*.java"
  - "**/graphql/**/*.java"
---

# GraphQL Backend Instructions

## Libraries and Frameworks
- **Java:** Programming language for backend implementation
- **Maven:** Build and dependency management tool
- **Spring Boot:** Web application framework for service configuration and dependency injection
- **Netflix DGS:** GraphQL server framework (`graphql-dgs-spring-boot-starter`, `graphql-dgs-codegen-shared-core`)
- **graphqlcodegen-maven-plugin:** Maven plugin for generating GraphQL schema classes and client code

## Resolver Implementation
- **Performance:** Consider potential N+1 problems and resolver performance
- **Caching strategy:** Consider caching implications for new fields and resolvers
- **Federation performance:** Evaluate impact on federated query execution
- **Security:** Operations relying on client-side input should **never** use walkable IDs

## Code Quality
- Ensure proper error handling in resolvers
- Implement appropriate validation for input parameters
- Follow consistent patterns for data fetching and transformation
