# API Versioning and Change Policy

This repository uses **Semantic Versioning** to communicate the
compatibility impact of changes to the API contract.

The API contract version is maintained in the root OpenAPI document:

```yaml
info:
  version: X.Y.Z
```

The version format is:

```text
MAJOR.MINOR.PATCH
```

Each part communicates the compatibility impact of a change:

- **MAJOR** --- breaking changes
- **MINOR** --- backward-compatible functionality
- **PATCH** --- backward-compatible fixes and corrections

---

## 1. PATCH version

Increment the PATCH version for changes that do not introduce new API
functionality or break existing consumers.

```text
0.1.0 → 0.1.1
```

Examples include:

- Correcting documentation
- Correcting descriptions
- Correcting examples
- Fixing typos
- Correcting metadata
- Fixing an implementation so that it behaves according to the
  existing contract
- Clarifying existing behavior without changing request or response
  semantics

### Example

Before:

```yaml
description: Returns all use records.
```

After:

```yaml
description: Returns a paginated list of users.
```

This is a PATCH change because the API contract behavior has not
changed.

---

## 2. MINOR version

Increment the MINOR version when adding new functionality in a
backward-compatible way.

```text
0.1.0 → 0.2.0
```

The PATCH version resets to zero when the MINOR version changes.

```text
1.4.3 → 1.5.0
```

Examples include:

- Adding a new endpoint
- Adding a new optional query parameter
- Adding a new optional request header
- Adding a new optional request body property
- Adding a new response property that existing consumers are not
  required to use
- Adding a new optional response header
- Adding a new operation
- Adding a new optional capability
- Marking an existing operation or field as deprecated

### Example: new endpoint

```text
GET /orders
```

Existing consumers are unaffected.

This is a MINOR change.

### Example: optional request property

Before:

```yaml
type: object

properties:
  name:
    type: string
```

After:

```yaml
type: object

properties:
  name:
    type: string

  displayName:
    type: string
```

If `displayName` is optional, existing clients can continue sending the
same request.

This is normally a MINOR change.

### Example: new response property

Before:

```json
{
  "id": "123",
  "name": "Jane Doe"
}
```

After:

```json
{
  "id": "123",
  "name": "Jane Doe",
  "createdAt": "2026-08-25T10:00:00Z"
}
```

Adding a response property is generally backward-compatible for properly
implemented clients.

This is normally a MINOR change.

---

## 3. MAJOR version

Increment the MAJOR version when a change can break an existing API
consumer.

```text
1.4.0 → 2.0.0
```

Both MINOR and PATCH reset to zero.

Examples include:

- Removing an endpoint
- Renaming an endpoint
- Removing an operation
- Removing a request property
- Removing a response property
- Renaming a property
- Changing a property type
- Changing a property format incompatibly
- Making an optional request property required
- Adding a new required request property
- Changing a parameter from optional to required
- Removing a query, path, or header parameter
- Changing parameter semantics incompatibly
- Removing a supported response status code
- Changing authentication requirements incompatibly
- Changing request or response formats incompatibly

### Example: renamed property

Before:

```json
{
  "name": "Jane Doe"
}
```

After:

```json
{
  "fullName": "Jane Doe"
}
```

Existing consumers expecting `name` will break.

This requires a MAJOR version increment.

### Example: optional to required

Before:

```yaml
type: object

properties:
  name:
    type: string
```

After:

```yaml
type: object

required:
  - name

properties:
  name:
    type: string
```

Existing consumers that previously omitted `name` may now fail
validation.

This is a breaking change and requires a MAJOR version increment.

---

# Current API maturity

The contract currently uses:

```text
0.x.y
```

For example:

```yaml
info:
  version: 0.1.0
```

Major version zero represents the initial development stage.

The contract is not yet considered stable, and the API may evolve more
rapidly before its first stable public release.

The first stable public release should be:

```text
1.0.0
```

At that point:

- The public API should be intentionally defined.
- Compatibility expectations should be clear.
- Breaking changes should require a new MAJOR version.

---

# Versioning workflow

Before merging a contract change:

## Step 1 --- Identify the change

Determine whether the change is:

```text
PATCH
MINOR
MAJOR
```

Ask:

> Can an existing API consumer continue working without changing its
> implementation?

If yes, the change may be PATCH or MINOR.

If no, it is a MAJOR change.

---

## Step 2 --- Update the API version

Update the version in:

```text
openapi/openapi.yaml
```

Example:

```yaml
info:
  version: 0.2.0
```

Do not change the version after a release has been published without
creating a new version.

---

## Step 3 --- Update the changelog

Document:

- What changed
- Why it changed
- Whether the change is breaking
- Any migration requirements

Example:

```text
Added:
- GET /users/{userId}

Changed:
- User response now includes createdAt

Deprecated:
- GET /legacy-users

Breaking:
- None
```

---

## Step 4 --- Run contract verification

Before committing or merging:

```bash
npm test
```

Then verify the documentation build:

```bash
npm run build:docs
```

The contract must pass the repository's quality checks before it is
merged.

---

# Deprecation policy

Breaking changes should not normally be introduced immediately when a
compatible migration path is available.

The preferred approach is:

```text
Existing API capability
        │
        ▼
Mark as deprecated
        │
        ▼
Introduce replacement
        │
        ▼
Document migration path
        │
        ▼
Allow consumers time to migrate
        │
        ▼
Remove in a future MAJOR version
```

For example:

```yaml
get:
  deprecated: true
  summary: Legacy user lookup
```

Then introduce a replacement operation.

Deprecation is preferable to immediate removal when consumers need a
migration period.

---

# Pre-release versions

Pre-release versions may be used when a contract is available for review
or integration but is not yet considered stable.

Examples:

```text
0.2.0-alpha
0.2.0-alpha.1
0.2.0-beta.1
0.2.0-rc.1
```

Recommended progression:

```text
0.2.0-alpha.1
       ↓
0.2.0-alpha.2
       ↓
0.2.0-beta.1
       ↓
0.2.0-rc.1
       ↓
0.2.0
```

Suggested meaning:

Version Meaning

---

`alpha` Early development
`beta` Feature-complete but still under validation
`rc` Release candidate
Final version Stable release

---

# API contract version vs OpenAPI version

These are different and must not be confused.

## OpenAPI Specification version

```yaml
openapi: 3.1.2
```

This defines the version of the **OpenAPI Specification format** used by
the document.

It determines which OpenAPI features and syntax the contract may use.

## API contract version

```yaml
info:
  version: 0.1.0
```

This defines the version of the **API being described**.

It communicates how the API contract itself evolves over time.

Therefore:

```text
openapi: 3.1.2
```

does **not** mean:

```text
API version 3.1.2
```

These version numbers serve different purposes.

---

# API versioning strategy

This repository defines how the **API contract version** evolves using
Semantic Versioning.

It does not force a specific API exposure strategy.

For example, an implementation may choose:

### URI versioning

```text
/v1/users
/v2/users
```

### Header versioning

```http
API-Version: 2
```

### Media type versioning

```http
Accept: application/vnd.example.users.v2+json
```

The appropriate strategy depends on the API and its consumers.

For this starter repository, the example paths remain version-neutral:

```text
/users
/users/{userId}
```

This avoids imposing a URL versioning strategy on every API built from
this template.

---

# Compatibility decision guide

Use this table when deciding which version to increment.

Change Version

---

Fix typo in description PATCH
Correct example data PATCH
Correct implementation to match existing contract PATCH
Add new endpoint MINOR
Add optional query parameter MINOR
Add optional request property MINOR
Add optional response property MINOR
Deprecate an operation MINOR
Remove endpoint MAJOR
Remove operation MAJOR
Remove property MAJOR
Rename property MAJOR
Change property type MAJOR
Make optional request property required MAJOR
Add required request property MAJOR
Change authentication incompatibly MAJOR
Remove supported response status MAJOR

When a change is unclear, prefer the version classification that most
safely communicates its potential impact to API consumers.

---

# Release policy

A released contract version should be treated as immutable.

Once a version is published, do not modify the contract under the same
version number.

Instead:

```text
Published version
        │
        ▼
Change required
        │
        ▼
Determine compatibility impact
        │
        ├── Compatible fix → PATCH
        │
        ├── Compatible feature → MINOR
        │
        └── Breaking change → MAJOR
```

Then publish the updated contract with the new version number.

---

# Future automation

This repository currently enforces:

```text
OpenAPI source
      │
      ▼
Lint
      │
      ▼
Reference resolution
      │
      ▼
Bundle
      │
      ▼
Documentation build
```

Future versions of this repository may add automated compatibility
checks between:

```text
Previous contract
        │
        ▼
Compatibility analysis
        │
        ▼
Current contract
```

This could automatically detect potentially breaking changes such as:

- Removed endpoints
- Removed operations
- Removed properties
- Changed property types
- New required request fields
- Changed parameter requirements

Such tooling should be added only when it provides a clear benefit and
fits the dependency and maintenance standards of the repository.

---

# Summary

```text
PATCH
│
└── Backward-compatible fixes and corrections

MINOR
│
└── Backward-compatible functionality

MAJOR
│
└── Breaking changes
```

The objective of this policy is simple:

> API consumers should be able to understand the expected compatibility
> impact of a release from its version number.

All API contract changes should therefore be evaluated for compatibility
before they are merged and released.
