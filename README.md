
# Mockilo

> **Mockilo** — The ultimate full‑stack mocking platform for developers. Instant fake data, mock APIs, auth, and seeding tools so you can build, test, and ship without waiting on backend infra.

---

## Table of contents

1. [What is Mockilo?](#what-is-mockilo)
2. [Key features](#key-features)
3. [Why use Mockilo?](#why-use-mockilo)
4. [Core components](#core-components)
5. [Quickstart](#quickstart)
6. [CLI reference (examples)](#cli-reference-examples)
7. [Configuration](#configuration)
8. [Example workflows](#example-workflows)
9. [Roadmap](#roadmap)
10. [Contributing](#contributing)
11. [License & Contact](#license--contact)

---

## What is Mockilo?

Mockilo is a developer-first toolkit for generating realistic mock data, running local mock APIs, simulating auth flows, and seeding databases — all from your schema. It combines the power of schema-aware generators with flexible CLI tooling and opinionated defaults so teams can prototype, test, and demo quickly.

Mockilo is designed for frontend engineers, backend devs, QA, and product teams who need reliable, repeatable mock data and realistic API behaviour without building production services.

---

## Key features

* Schema-aware mock data generation (Prisma, JSON Schema, or custom model files).
* Local mock API server that maps endpoints to schema-driven responses.
* Fake auth layer for development/staging (login, tokens, RBAC simulation).
* Deterministic seeding with `--seed` and `--seed-consistent` for reproducible datasets.
* CLI first: generate, serve, seed, and snapshot data with single commands.
* Customizable generators and presets (`mock.config.js` / `mock-config.json`).
* Output formats: JSON, SQL INSERT, CSV, and fixtures for test runners.
* Relation-aware generation with circular relation handling and depth control.
* **Live API mocking with hot-reload** — edit schemas and instantly see updated responses.
* **Preset library** — share and import mock data configurations for common models.

---

## Why use Mockilo?

* Save time: avoid hand-writing fixtures or building throwaway APIs.
* Improve DX: frontends can develop against stable, realistic datasets.
* Reduce flakiness: deterministic seeds and consistent relations make tests reliable.
* Ship faster: demos and prototypes look production-like with minimal effort.
* Boost collaboration: same mock data across frontend, backend, and QA teams.

---

## Core components

* **Mocktail (CLI)** — Schema-aware mock data generator. CLI-first with flags for model selection, counts, depth, output format, and seeding.
* **SchemaGhost** — Instant local mock API server that exposes REST/GraphQL endpoints generated from your schema.
* **MockAuth** — Simple dev/staging authentication layer to simulate sign-in flows, roles, and permissions.

> These pieces can be used independently or together as a development stack.

---

## Quickstart

### Install (local / dev)

```bash
# install the CLI globally (optional)
npm i -g @mockilo/cli

# or use npx for one-off usage
npx @mockilo/cli generate --help
```

### Generate mock data from a Prisma schema

```bash
npx @mockilo/cli generate \
  --schema ./prisma/schema.prisma \
  --models User,Post \
  --count 50 \
  --output ./mocks/data.json \
  --seed
```

* `--depth 2` — controls how deeply relations are expanded.
* `--seed` — optionally write results into your database (supports Prisma Client, SQL files).

---

## CLI reference (examples)

```
# Generate 10 Users and 30 Posts and write to one file
mockilo generate --schema ./prisma/schema.prisma --models User,Post --count 10,30 --output mocks.json

# Run the mock API server using the generated mock file
mockilo serve --data ./mocks.json --port 4010

# Start a dev auth server
mock-verse auth --strategy simple --users ./auth/users.json
```

See `mockilo generate --help` for full options.

---

## Configuration

Place a `mock.config.js` (or `mock.config.json`) at your project root to customize generation rules, per-model generators, presets, or seeding hooks.

Example `mock.config.js`:

```js
module.exports = {
  defaults: { locale: 'en', seedConsistency: true },
  models: {
    User: { count: 50, faker: { name: 'fullName', email: 'email' } },
    Post: { count: 200, relations: { author: { connectBy: 'User' } } }
  }
}
```

---

## Example workflows

### Frontend prototyping

1. Generate a dataset from your schema: `mockilo generate --schema ./schema.prisma --count 50`
2. Serve it locally: `mockilo serve --data ./mocks.json`
3. Point frontend to `http://localhost:4010` and build the UI.

### End-to-end testing with deterministic seeds

1. Generate deterministic seed: `mockilo generate --seed --seed-consistent --seed-value 12345`
2. Run tests using the seeded DB state.

---

## Roadmap (high level)

* v0.x: Core CLI, schema parsing, schema-aware generators, local server
* v1.0: GUI dashboard (web), team sharing, presets marketplace
* v2.0+: Cloud-based seeding, multi-environment management, enterprise features

We prioritize developer DX, performance, and community contributions.

---

## Contributing

We ❤️ open source. Whether you file issues, send PRs, or help with docs — contributions matter.

1. Fork the repo
2. Open an issue describing the feature/bug
3. Send a focused PR with tests and docs

Please follow the code style and run the test suite before submitting PRs.

---

## License & Contact

Mockilo is BSL1.1 licensed.

Have ideas, questions, or want to collaborate? Open an issue or reach out on the project repository.

---

*Made with ❤️ for developers who want to build fast.*
