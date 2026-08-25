# OpenAPI API Contract Starter

A practical OpenAPI-first foundation for designing, documenting, and validating HTTP API contracts.

This project demonstrates a modular approach to API contract design with reusable schemas, standardized error responses, interactive documentation, validation, and continuous integration.

## Status

🚧 Under active development.

## Goals

- OpenAPI-first API design
- Reusable contract components
- Consistent HTTP responses
- Standardized API errors
- Interactive documentation with Swagger UI
- Automated contract validation

## Project structure

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
