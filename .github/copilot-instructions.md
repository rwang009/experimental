# Copilot Instructions

This document provides structured review instructions for different types of code changes to ensure consistency, quality, and adherence to best practices across the codebase. Use your own judgment if not listed in the instructions.

## GraphQL Schema Review Instructions

**Goal:** Ensure GraphQL schema changes maintain backward compatibility, follow Parchment's naming conventions, provide comprehensive documentation, and avoid performance pitfalls while adhering to Apollo Federation best practices.

---
applyTo:
  - "**/*.graphql"
  - "**/*.gql"
  - "**/*graphql*"
  - "**/*fetcher*.java"
  - "**/*datafetcher*.java"
  - "**/dgs/**/*.java"
  - "**/graphql/**/*.java"
---

**Focus Areas:** 

#### Schema Design & Naming Conventions
- **Avoid common names in schemas** - use descriptive, domain-specific names
- **For service-specific types:** Prefix with service name following pattern: `domain-prefix + subdomain-prefix (optional) + typename`
  - Example: `FulfillmentWorkQueueDocumentType` instead of `DocumentType`
- **For global data:** Use standard names (e.g., `OrderLineItem`, `Member`) that are consistent across domains
- **Federation directives:** Use `@key` for entities or `@shareable` for generic types across domains
- **Interface naming:** Don't use `Interface` suffix - (e.g., use `interface OrderLineItem` instead of `interface OrderLineItemInterface`)
- **Input/Output naming:** Follow hierarchy: `Criteria` → `Input` → `Request/Response`
  - `Criteria` for search endpoints
  - `Input` for updates/creates when separate from search
  - `Request/Response` for composed types

#### Backward Compatibility & Migration Strategy
- **Critical:** Changing existing field names breaks backward compatibility
- **Impact analysis:** Use Apollo Studio → Insights tab to check metrics for operations, fields, and enums
- **Breaking change process:**
  1. Add new fields alongside existing ones
  2. Migrate client usages from old to new fields  
  3. Remove old fields only after confirming all usages are dropped
- **Migration planning:** Design comprehensive migration strategy for any breaking changes

#### Type Safety & Field Validation
- **Unique identifiers:** Use `type ID` for unique identifiers in schema definitions. This rule is excluded for Input types.
- **Input types:** Use actual types (e.g., `UUID`, `String`, `Int`) instead of generic `type ID` for Input definitions
- **Security:** Operations relying on client-side input should **never** use walkable IDs
- **Field requirements:** Carefully evaluate required (`!`) vs optional fields based on business logic
- **Federation entities:** Properly implement `@key` directives for federated entities

#### Documentation Requirements
- **Required documentation strings** for non-obvious field names or fields with non-standard configuration
- **Documentation format:** Use `"""` syntax for all documentation strings
- **Clarity:** Provide clear descriptions that help unfamiliar users understand domain concepts
- **Deprecation:** Mark deprecated fields with `@deprecated` directive and include clear migration reasons

#### Performance & Optimization Considerations
- **Expensive fields:** Mark with appropriate caution or optimize (e.g., recommend `useLazyQuery`)
- **Query complexity:** Consider potential N+1 problems and resolver performance
- **Federation performance:** Evaluate impact on federated query execution
- **Caching strategy:** Consider caching implications for new fields and resolvers

#### Interface Design
- **Interface usage criteria:** Use interfaces when types should be tightly coupled to the abstraction
- **Field inheritance:** Manually include all interface fields in implementing types
- **Additional fields:** Implementing types can include fields beyond interface definition
- **Querying:** Ensure proper inline fragment usage for interface-specific fields

**Risks:** 
- Breaking existing client queries/mutations without proper migration
- Performance degradation from expensive resolvers or complex nested queries
- Security vulnerabilities from improperly validated inputs or exposed walkable IDs
- Federation conflicts from naming collisions or improper directive usage
- Documentation gaps that make API consumption difficult

**Testing Requirements:** 
- Validate schema compilation and federation compatibility
- Test backward compatibility with existing client queries
- Verify resolver performance under realistic load conditions
- Include integration tests for federated schema changes across subgraphs
- Test deprecated field warnings and migration paths
- Validate documentation string generation in Apollo Studio
- Test interface inheritance and inline fragment querying

**Additional Validation:**
- Confirm adherence to Type Naming Conventions documentation
- Verify Documentation Strings guidelines compliance
- Check Apollo Federation entity relationships
- Validate input/output object naming hierarchy
- Ensure proper interface design patterns
