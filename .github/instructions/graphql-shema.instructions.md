---
applyTo: "**/*.graphql"
---

# GraphQL Schema Instructions

## Naming Conventions
- **Service-specific types:** Prefix with `domain + subdomain + typename` (e.g., `FulfillmentWorkQueueDocumentType` not `DocumentType`)
- **Interfaces:** No `Interface` suffix (use `OrderLineItem` not `OrderLineItemInterface`)
- **Input hierarchy:** `Criteria` (search) → `Input` (updates/creates) → `Request/Response` (composed types)
- **Enums:** Use descriptive, domain-specific enum names and values

## Type Safety
- **Unique identifiers:** Use `type ID` in schema definitions (except Input types use concrete types: `UUID`, `String`, `Int`)
- **Required fields:** Carefully evaluate `!` vs optional based on business logic
- **Walkable IDs:** Never use walkable IDs for client-side input operations

## Backward Compatibility
- **Breaking changes:** Add new fields alongside existing ones, migrate usage, then remove old fields
- **Deprecation:** Include clear migration reasons with `@deprecated`

## Interface Design
- **Interface usage:** Use interfaces only when types should be tightly coupled to abstraction
- **Field inheritance:** Manually include all interface fields in implementing types (types can add additional fields beyond the interface)

## Federation
- **Cross-domain types:** Use consistent standard names across domains (e.g., `OrderLineItem`, `Member`) and mark with appropriate directives
- **Entity directives:** Use `@key` for federated entities, `@shareable` for types shared across services
- **Federation impact:** Evaluate federated query execution performance

## Documentation
- **Required documentation:** Use `"""` syntax for non-obvious fields and domain concepts
- **Performance warnings:** Document expensive fields with performance warnings (e.g., "Heavy operation - consider using lazy loading")
- **Deprecation:** Include clear migration reasons with `@deprecated`

## Performance
- **N+1 prevention:** Consider resolver performance and caching strategy
- **Expensive operations:** Mark heavy computation or external calls in field descriptions


# Apollo Linter Rules

## Naming Rules

- DIRECTIVE_NAMES_SHOULD_BE_CAMEL_CASE: Directive names should always use camelCase.

  **Correct code:**
  ```graphql
  # camelCase
  directive @specialField on FIELD_DEFINITION
  ```

  **Incorrect code:**
  ```graphql
  # PascalCase
  directive @SpecialField on FIELD_DEFINITION
  ```

- ENUM_PREFIX: An enum type's name should never use the prefix `Enum`.

  **Correct code:**
  ```graphql
  enum Residence {
    HOUSE
    APARTMENT
    CONDO
  }
  ```

  **Incorrect code:**
  ```graphql
  enum EnumResidence {
    HOUSE
    APARTMENT
    CONDO
  }
  ```

- ENUM_SUFFIX: An enum type's name should never use the suffix `Enum`.

  **Correct code:**
  ```graphql
  enum Residence {
    HOUSE
    APARTMENT
    CONDO
  }
  ```

  **Incorrect code:**
  ```graphql
  enum ResidenceEnum {
    HOUSE
    APARTMENT
    CONDO
  }
  ```

- ENUM_USED_AS_INPUT_WITHOUT_SUFFIX: If an enum type is used as an input argument, its name should use the suffix `Input`.

  **Correct code:**
  ```graphql
  enum RoleInput {
    EDITOR
    VIEWER
  }

  type Query {
    users(role: RoleInput): [User!]!
  }
  ```

  **Incorrect code:**
  ```graphql
  enum Role {
    EDITOR
    VIEWER
  }

  type Query {
    users(role: Role): [User!]!
  }
  ```

- ENUM_USED_AS_OUTPUT_DESPITE_SUFFIX: If an enum is used as the return type of a non-input field, its name should not use the suffix `Input`.

  **Correct code:**
  ```graphql
  enum Role {
    EDITOR
    VIEWER
  }

  type Query {
    userRole(userId: ID!): Role
  }
  ```

  **Incorrect code:**
  ```graphql
  enum RoleInput {
    EDITOR
    VIEWER
  }

  type Query {
    userRole(userId: ID!): RoleInput
  }
  ```

- ENUM_VALUES_SHOULD_BE_SCREAMING_SNAKE_CASE: Enum values should always use SCREAMING_SNAKE_CASE.

  **Correct code:**
  ```graphql
  enum Amenity {
    # SCREAMING_SNAKE_CASE 😱
    PUBLIC_PARK
  }
  ```

  **Incorrect code:**
  ```graphql
  enum Amenity {
    # snake_case
    public_park
  }
  ```

- FIELD_NAMES_SHOULD_BE_CAMEL_CASE: Field names should always use camelCase.

  **Correct code:**
  ```graphql
  type User {
    # camelCase
    firstName: String
  }
  ```

  **Incorrect code:**
  ```graphql
  type User {
    # PascalCase
    FirstName: String!
  }
  ```

- INPUT_ARGUMENT_NAMES_SHOULD_BE_CAMEL_CASE: A GraphQL argument's name should always use camelCase.

  **Correct code:**
  ```graphql
  type Mutation {
    # camelCase
    createBlogPost(blogPostContent: BlogPostContent!): Post
  }
  ```

  **Incorrect code:**
  ```graphql
  type Mutation {
    # PascalCase
    createBlogPost(BlogPostContent: BlogPostContent!): Post
  }
  ```

- INTERFACE_PREFIX: An interface type's name should never use the prefix `Interface`.

  **Correct code:**
  ```graphql
  interface Book {
    title: String
    author: String
  }
  ```

  **Incorrect code:**
  ```graphql
  interface InterfaceBook {
    title: String
    author: String
  }
  ```

- INTERFACE_SUFFIX: An interface type's name should never use the suffix `Interface`.

  **Correct code:**
  ```graphql
  interface Book {
    title: String
    author: String
  }
  ```

  **Incorrect code:**
  ```graphql
  interface BookInterface {
    title: String
    author: String
  }
  ```

- OBJECT_PREFIX: An object type's name should never use the prefix `Object`.

  **Correct code:**
  ```graphql
  type Book {
    title: String!
  }
  ```

  **Incorrect code:**
  ```graphql
  type ObjectBook {
    title: String!
  }
  ```

- OBJECT_SUFFIX: An object type's name should never use the suffix `Object`.

  **Correct code:**
  ```graphql
  type Book {
    title: String!
  }
  ```

  **Incorrect code:**
  ```graphql
  type BookObject {
    title: String!
  }
  ```

- TYPE_NAMES_SHOULD_BE_PASCAL_CASE: All type names should use PascalCase.

  **Correct code:**
  ```graphql
  type StreamingService {
    # PascalCase
    id: ID!
  }
  ```

  **Incorrect code:**
  ```graphql
  type streamingService {
    # camelCase
    id: ID!
  }
  ```

- TYPE_PREFIX: Type names should never use the prefix `Type`.

  **Correct code:**
  ```graphql
  interface Book {
    title: String!
  }
  ```

  **Incorrect code:**
  ```graphql
  interface TypeBook {
    title: String!
  }
  ```

- TYPE_SUFFIX: Type names should never use the suffix `Type`.

  **Correct code:**
  ```graphql
  interface Book {
    title: String!
  }
  ```

  **Incorrect code:**
  ```graphql
  interface BookType {
    title: String!
  }
  ```


## Other Rules

- CONTACT_DIRECTIVE_MISSING: A subgraph schema should always provide owner contact details via the @contact directive.

  **Correct code:**
  ```graphql
  "Annotate a schema with contact information for the subgraph owner"
  directive @contact(
    name: String!
    url: String
    description: String
  ) on SCHEMA

  extend schema @contact(
    name: "Products Team",
    url: "https://myteam.slack.com/teams-chat-room-url",
    description: "Send urgent issues to #oncall."
  )
  ```

- DEFINED_TYPES_ARE_UNUSED: Every type defined in a schema should be used at least once. Unused types commonly appear after you refactor other parts of your schema. You should remove unused types to prevent confusion.

  **Correct code:**
  ```graphql
  type Book {
    title: String!
  }

  type Query {
    books: [Book!]!
  }
  ```

  **Incorrect code:**
  ```graphql
  type SomeUnusedType { # Also fails the TYPE_SUFFIX rule!
    name: String!
  }

  type AnActuallyUsedType {
    name: String!
  }

  type Query {
    hello: String!
    title: AnActuallyUsedType
  }
  ```

- DEPRECATED_DIRECTIVE_MISSING_REASON: The @deprecated directive should always include a reason argument.

  **Correct code:**
  ```graphql
  type Product {
    title: String @deprecated(reason: "Use Product.name instead")
    name: String!
  }
  ```

  **Incorrect code:**
  ```graphql
  type Product {
    title: String @deprecated
    name: String!
  }
  ```


