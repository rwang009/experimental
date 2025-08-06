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
