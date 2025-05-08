# System Patterns & Conventions (SMCP)
*Purpose: Documents recurring design patterns, coding standards, and architectural choices specific to the SMCP project.*
*Updates: Appended or refined by AI/user as patterns emerge or standards are set.*
*Last Reviewed: 2025-05-09*
---
## Coding Style / Linting
*   **Linter:** Ruff (preferred) or Flake8
*   **Formatter:** Black (mandatory)
*   **Style Guide:** PEP8 adherence
*   **Docstrings:** Google style (Mandatory for public APIs, especially for SMCP examples and platform code)
*   **Type Hinting:** Mandatory for function signatures (especially for SMCP examples and platform code)
*   **General:** [Define key rules, formatting standards for examples and platform code as they evolve]

## Documentation Standards (Living Docs)
*   [e.g., Structure for tutorials, READMEs for SMCP examples and modules]
*   [e.g., Guidance on using "Edit on GitHub" links or similar contribution methods for documentation]
*   [e.g., Process for updating documentation to keep it current with SMCP evolution]

## Common Data Structures
*   [e.g., Standard format for MCP Server Catalog entries (JSON Schema to be defined)]
*   [e.g., API response structures for the SMCP platform (e.g., using Pydantic for validation)]

## Architectural Patterns (SMCP)
*   **Educational Examples Structure:** [e.g., Clear client/server separation, comprehensive READMEs, use of project templates]
*   **MCP Server Catalog Design:** [e.g., Database schema considerations (PostgreSQL), API design principles (RESTful)]
*   **Agentic System (Creator) Design:** [e.g., Mixture-of-Experts (MoE) component interaction, template definition language (YAML/JSON), output structure for containerized systems]
*   **General:** [e.g., Consider patterns like MVC, Microservices if/as applicable to SMCP platform components]

## Naming Conventions
*   **Variables/Functions:** `snake_case`
*   **Constants:** `UPPER_SNAKE_CASE`
*   **Classes:** `PascalCase`
*   **Files:** `kebab-case` or `snake_case` (maintain consistency within modules/components of SMCP)
*   **Repository Names:** [e.g., `smcp-core`, `smcp-docs-and-examples` - follow established patterns]

## Error Handling Strategy
*   [e.g., Define strategy for SMCP example applications - how to demonstrate good error handling]
*   [e.g., Define strategy for the SMCP platform services - centralized error handling, specific exception types]

## Security Considerations (SMCP Core Principle)
*   **General:** Enforce input validation, secure secrets management (e.g., `.env` for local dev, Vault for production), regular dependency scanning for all SMCP code (examples & platform).
*   **MCP Communications:** Document recommendations for mTLS (Mutual TLS) using X.509 certificates (potentially via SecureG PKI), and message signing for integrity and non-repudiation.
*   **Platform Security:** Adhere to secure-by-design principles for the MCP Server Catalog and the Agentic System Creator. Vet all templates and generated code patterns for security.

## Deployment Patterns
*   **SMCP Examples:** [e.g., Standardized Dockerfile strategy for examples, clear local run instructions, considerations for easy deployment by users]
*   **SMCP Platform (Future):** [e.g., Containerization (Docker, Compose, K8s), CI/CD pipeline notes, infrastructure as code practices]
