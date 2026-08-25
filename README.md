# OpenAPI API Contract Starter

A practical OpenAPI-first foundation for designing, documenting, and validating HTTP API contracts.

This project demonstrates a modular approach to API contract design with reusable schemas, standardized error responses, interactive documentation, validation, and continuous integration.

The repository demonstrates a contract-first approach using OpenAPI, reusable components, Swagger UI, Redocly linting, and GitHub Actions.

## What this repository provides

- Modular OpenAPI structure
- Reusable schemas, parameters, and responses
- Standardized problem and validation error models
- JWT bearer authentication definition
- Example API operations
- Interactive Swagger UI
- OpenAPI linting with Redocly
- Contract verification and bundling
- GitHub Actions CI

## Repository structure

```text
├── README.md
├── openapi
│   ├── components
│   │   ├── parameters
│   │   │   ├── page.yaml
│   │   │   ├── size.yaml
│   │   │   └── user-id.yaml
│   │   ├── responses
│   │   │   ├── bad-request.yaml
│   │   │   ├── conflict.yaml
│   │   │   ├── forbidden.yaml
│   │   │   ├── internal-error.yaml
│   │   │   ├── not-found.yaml
│   │   │   └── unauthorized.yaml
│   │   ├── schemas
│   │   │   ├── create-user-request.yaml
│   │   │   ├── health-response.yaml
│   │   │   ├── page-metadata.yaml
│   │   │   ├── problem.yaml
│   │   │   ├── user-page.yaml
│   │   │   ├── user.yaml
│   │   │   └── validation-error.yaml
│   │   └── security
│   │       └── bearer-auth.yaml
│   ├── openapi.yaml
│   └── paths
│       ├── health.yaml
│       ├── user-by-id.yaml
│       └── users.yaml
├── package-lock.json
├── package.json
├── redocly.yaml
└── swagger-ui
    └── swagger-initializer.js
```

## Quick Start

1. **Clone the repository:**

   ```bash
   git clone https://github.com/allydevs-engineering/openapi-api-contract-starter.git
   cd openapi-api-contract-starter
   ```

2. **Install project dependencies:**

   ```bash
   npm ci
   ```

3. **Validate and bundle the API contract:**

   ```bash
   npm run test
   ```

4. **Build and run the API documentation:**

   ```bash
   npm run docs
   ```

   Open [http://localhost:3000](http://localhost:3000) in your browser.

## 📦 Available Commands

Execute these commands from the root directory using your terminal:

| Command              | Purpose                                  |
| :------------------- | :--------------------------------------- |
| `npm run lint`       | Lint the OpenAPI contract using Redocly. |
| `npm run validate`   | Validate the contract.                   |
| `npm run bundle`     | Bundle the modular OpenAPI files.        |
| `npm run test`       | Run contract verification.               |
| `npm run build:docs` | Build the bundled OpenAPI documentation. |
| `npm run docs`       | Start Swagger UI locally.                |

---

## Contract Structure

The API is intentionally split into smaller files instead of maintaining one large OpenAPI document.

```text
openapi/
├── openapi.yaml
├── paths/
└── components/
    ├── schemas/
    ├── responses/
    ├── parameters/
    └── security/
```

The root `openapi.yaml` acts as the entry point and references paths and reusable components using `$ref`.
This structure keeps the contract easier to evolve as the number of endpoints and schemas grows. OpenAPI descriptions are designed to support tooling such as documentation, client/server generation, and testing.

## Adding a new endpoint

1. Create a new path definition:

```text
openapi/paths/orders.yaml
```

2. Define the operations and responses.
3. Add reusable schemas, parameters, or responses under `components/` when appropriate.
4. Reference the path from `openapi/openapi.yaml`.

Example:

```text
paths:
  /orders:
    $ref: ./paths/orders.yaml
```

5. Run:

```bash
npm test
```

6. Verify the documentation:

```bash
npm run docs
```

## Quality checks

The contract is checked locally and in GitHub Actions.

```text
OpenAPI source
      │
      ▼
Redocly lint
      │
      ▼
Reference resolution and bundling
      │
      ▼
Swagger documentation build
```

The CI workflow runs these checks on pushes and pull requests targeting `main`.

## Swagger UI

Swagger UI provides interactive API documentation based on the bundled OpenAPI contract.

The current repository is contract-only and does not include an API implementation.

As a result, the documentation can display operations, schemas, request bodies, and responses, but `Try it out` requires a compatible API server to be available.

If Swagger UI and the API server are hosted on different origins, the API must allow the browser request through CORS.

## Using this repository as a starter

This repository can be used as a foundation for a new API contract.

Typical workflow:

```text
1. Define the API contract
        ↓
2. Add reusable schemas and responses
        ↓
3. Validate with npm test
        ↓
4. Review with Swagger UI
        ↓
5. Implement the API
        ↓
6. Keep the implementation aligned with the contract
```

## Contributing

Contributions, improvements and discussions are welcome.
Before opening a pull request:

- Run formatting checks.
- Run linting.
- Run the test suite.
- Ensure the production build succeeds.
- Use a Conventional Commit message.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for development and contribution guidelines.

## Security

Please review [SECURITY.md](./SECURITY.md) for information about reporting security vulnerabilities.

## Maintained by

AllyDevs Engineering

Engineering capability for digital agencies.

Website: [https://allydevs.com](https://allydevs.com)

## License

This project is licensed under the MIT License. See [LICENSE](./LICENSE).
