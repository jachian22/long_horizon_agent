# Naming Conventions (Draft)

This document defines human-readable naming conventions for Modal apps, secrets, and local artifacts.

---

## Modal App Names

Use explicit, human-readable app names to separate environments:

- `long-horizon-agent-dev`
- `long-horizon-agent-prod`

---

## Modal Secret Names

Keep the existing prefix standard and add an environment indicator.

Format:

```
LONG_HORIZON_AGENT_<ENV>_<SOURCE>_<TYPE>
```

Examples:

- `LONG_HORIZON_AGENT_DEV_CHASE_OAUTH`
- `LONG_HORIZON_AGENT_PROD_AMEX_SESSION`

---

## Volume Names

Use environment-prefixed volumes to avoid accidental cross-environment access:

- `long-horizon-dev-pending-specs`
- `long-horizon-dev-pending`
- `long-horizon-dev-approved`
- `long-horizon-dev-exports`
- `long-horizon-dev-runs`

- `long-horizon-prod-pending-specs`
- `long-horizon-prod-pending`
- `long-horizon-prod-approved`
- `long-horizon-prod-exports`
- `long-horizon-prod-runs`

---

## Source Identifiers

- Use short, stable, lowercase identifiers (e.g., `chase`, `amex`, `plaid`).
- Prefer official brand names when ambiguous.
- Avoid spaces or punctuation.

---

## Job Identifiers

- Use UUIDs or stable job slugs.
- Prefer UUIDs for auditability (avoid collisions).

---

## Run Identifiers

- Use a timestamp-based format to aid chronological sorting.
- Example: `run_2024_05_12_001`.

