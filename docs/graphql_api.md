# GraphQL API Overview

This document provides an overview of the GraphQL architecture used by the Ruby on Rails–backed single-page application (SPA) for the online shopping platform. It covers API visibility, feature gating, tenancy, best practices, and request context.

---

## Architecture Overview

The online shopping SPA communicates with a GraphQL API served by a Ruby on Rails backend. Key components include:

* **GraphQL Ruby**: Provides schema definition, query execution, and relay-style patterns.
* **SPA Frontend**: A JavaScript-based client (React/Vue/etc.) using a GraphQL client (e.g., Apollo) to fetch and mutate data.
* **Domain Modules**: Products, users, carts, orders, and payments each expose GraphQL types and resolvers.
* **Background Jobs**: Used for asynchronous tasks related to checkout, email delivery, and inventory updates.
* **Caching Layer**: Optional memoization within resolvers plus HTTP-level caching for persisted queries.

---

## API Visibility

The GraphQL schema is versioned and designed with controlled visibility:

* **Public Types** expose product catalog, pricing, and authentication flows.
* **Private Types** include administrative or internal fields not intended for external consumption.
* **Field-Level Visibility** allows hiding fields based on authentication, role, or experimental availability.
* **Deprecated Fields** remain in schema but are marked with instructions for migration.

Backend logic ensures:

* **Unauthenticated users** only see public product data.
* **Authenticated customers** access cart, checkout, and order history.
* **Admins** access internal fields such as inventory or user management.

---

## Feature Gating

Feature flags ensure safe rollout of new API capabilities.

* **Boolean Gates:** Enable/disable entire fields or mutations.
* **User-Targeted Gates:** Activate features for a percentage of users or specific segments.
* **Environment Gates:** Allow beta features in staging but not production.
* **Resolver Hooks:** Check feature flags at runtime to allow/deny fields or override behavior.

Implementation may use libraries such as Flipper or LaunchDarkly.

---

## Tenancy

The API supports multi-tenant behavior when hosting multiple storefronts or merchant instances.

* **Tenant Identification:** Extracted from request headers, tokens, or subdomain.
* **Scoped Queries:** Product, user, and order data are restricted to the active tenant.
* **Dedicated Context:** Tenant is injected into GraphQL context for resolvers.
* **Data Isolation:** All mutations and reads enforce tenant-level scoping via database filters.

---

## Best Practices

To ensure predictable and maintainable GraphQL behavior:

* **Avoid Heavy Resolvers:** Keep resolvers thin; push business logic into service objects.
* **Use `batch_loaders` or `dataloaders`:** Prevent N+1 database queries.
* **Validate Inputs:** Use custom scalar types and input validation for mutations.
* **Paginate Regularly:** Use cursor-based pagination for large lists.
* **Strict Nullability:** Clearly define which fields may return `null`.
* **Type-Level Authorization:** Avoid authorization checks spread across resolvers.
* **Log Query Metadata:** Including user ID, operation name, and execution time.

---

## GraphQL Context

Every GraphQL request receives a context object that includes:

* **Current User**: Derived from sessions or tokens.
* **Tenant**: Identified through request metadata.
* **Feature Flags**: Used for dynamic schema behavior.
* **Request Metadata**: IP address, headers, and client identifiers.
* **Tracing Handlers**: May plug into application performance monitoring.

Context is the foundation for authentication, authorization, feature gating, and tenancy enforcement.
