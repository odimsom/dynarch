<div align="center">

# Dynarch

**Metadata-driven architecture, plug and play.**

A framework to design, connect and execute data pipelines through metadata — no hardcoded logic.

[![License](https://img.shields.io/badge/license-Proprietary%20%2F%20Revenue%20Share-red.svg)](#license)
[![Python](https://img.shields.io/badge/python-3.11%2B-blue.svg)](packages/dynarch-py)
[![TypeScript](https://img.shields.io/badge/typescript-5.x-blue.svg)](packages/dynarch-ts)
[![NestJS](https://img.shields.io/badge/nestjs-11.x-red.svg)](apps/dynarch-api)
[![SvelteKit](https://img.shields.io/badge/sveltekit-2.x-orange.svg)](apps/dynarch-ui)
[![Self-hosted](https://img.shields.io/badge/self--hosted-docker-informational.svg)](#self-hosted)

</div>

---

## What is Dynarch?

Most software embeds business rules, field validations, and workflow logic directly into the source code. Changing anything means rewriting, recompiling, and redeploying.

**Dynarch flips that model.**

In a metadata-driven architecture, the code is a generic engine. It doesn't know what to do until it reads a configuration file — the metadata — that describes the behavior at runtime. Add a field, change a rule, or rewire a flow by editing a JSON file. No code changes. No redeployment.

The problem is that implementing this architecture from scratch is notoriously hard. Existing tools like Apache Hop or Airbyte solve enterprise-scale problems, but they're complex, heavy, and intimidating for anyone trying to learn or prototype.

**Dynarch is the missing middle ground:** a lightweight, self-hosted platform that lets you understand, implement, and operate metadata-driven architecture without dying in the attempt.

---

## How it works

```
┌─────────────┐     pipeline.json      ┌──────────────┐     ┌─────────────┐
│   Source    │ ──────────────────────▶│    Engine    │────▶│   Target    │
│ (DB / CSV)  │                        │ (reads meta) │     │ (DB / JSON) │
└─────────────┘                        └──────────────┘     └─────────────┘
                                               ▲
                                               │
                                     ┌─────────────────┐
                                     │  dynarch-ui     │
                                     │  (visual wizard)│
                                     └─────────────────┘
```

1. **Connect** your source — a database, a CSV, or a JSON file
2. **Configure** the rules through a step-by-step visual interface (or write the `pipeline.json` directly)
3. **Execute** — the engine reads the metadata and moves, transforms, and validates your data
4. **Automate** — expose pipelines as MCP tools so AI agents can trigger them via natural language

---

## Core concepts

| Concept | Description |
|---|---|
| **Pipeline** | A complete data flow definition stored as a `pipeline.json` file |
| **Source** | Where the data comes from (database table, CSV, API) |
| **Rules** | Transformations and validations applied to each field |
| **Target** | Where the processed data goes (database, file, another system) |
| **Engine** | The generic runtime that reads and executes any valid pipeline |

---

## Monorepo structure

```
dynarch/
├── apps/
│   ├── dynarch-api/        # NestJS REST API — orchestrates everything
│   └── dynarch-ui/         # SvelteKit — visual step-by-step pipeline builder
│
├── packages/
│   ├── dynarch-ts/         # Core engine (TypeScript) — publish to npm
│   └── dynarch-py/         # Core engine (Python) — publish to PyPI
│
├── examples/
│   ├── csv-to-json/        # Read a CSV, apply rules, export to JSON
│   ├── db-to-csv/          # Connect a DB, extract a table, export to CSV
│   └── mcp-agent/          # Pipeline triggered by an AI agent via MCP
│
└── docs/
    ├── getting-started.md
    ├── architecture.md
    ├── mcp-integration.md
    └── api-reference.md
```

---

## The pipeline format

Everything in Dynarch is driven by a single JSON file. This is what a basic pipeline looks like:

```json
{
  "version": "1.0",
  "pipeline_name": "clean_users",
  "source": {
    "type": "csv",
    "path": "data/users.csv"
  },
  "rules": [
    { "field": "email",  "action": "lowercase" },
    { "field": "name",   "action": "uppercase" },
    { "field": "phone",  "action": "strip_spaces" }
  ],
  "target": {
    "type": "json_file",
    "path": "output/users_clean.json"
  }
}
```

The same file works with both the Python and TypeScript engines.

---

## Self-hosted

Dynarch runs entirely on your infrastructure. No cloud account required.

```bash
git clone https://github.com/odimsom/dynarch.git
cd dynarch
docker compose up
```

Open `http://localhost:3000` — that's it.

---

## Use as a library

**TypeScript / Node.js**

```bash
npm install dynarch-ts
```

```typescript
import { DynarchEngine } from 'dynarch-ts';

const engine = new DynarchEngine('pipeline.json');
await engine.run();
```

**Python**

```bash
pip install dynarch
```

```python
from dynarch import DynarchEngine

engine = DynarchEngine(config_path="pipeline.json")
engine.run()
```

---

## MCP Integration

Dynarch exposes three MCP tools that any AI agent can call:

| Tool | Description |
|---|---|
| `analyze_db` | Connect to a database and extract its schema |
| `generate_pipeline` | Build a `pipeline.json` from natural language instructions |
| `execute_pipeline` | Run a pipeline by name and return the result |

This means you can tell your AI assistant: *"Connect to my local database, clean the users table, and export it to CSV"* — and Dynarch handles the rest.

See [docs/mcp-integration.md](docs/mcp-integration.md) for setup details.

---

## Roadmap

- [ ] Core engine — Python
- [ ] Core engine — TypeScript
- [ ] NestJS API with schema introspection
- [ ] SvelteKit visual pipeline builder
- [ ] Docker self-hosted setup
- [ ] MCP server with 3 core tools
- [ ] npm package release
- [ ] PyPI package release
- [ ] Support for PostgreSQL, MySQL, SQLite
- [ ] Support for REST API sources

---

## License

Internal and educational use is free.  
Commercial use or revenue-generating activities require a commercial license.

See [LICENSE](LICENSE) and [LICENSE-COMMERCIAL](LICENSE-COMMERCIAL) for details.

---

<div align="center">
Built with intention by <a href="https://github.com/AlexanderMatos01Dev">@AlexanderMatos01Dev</a> & <a href="https://github.com/odimsom">@odimsom</a>
</div>
