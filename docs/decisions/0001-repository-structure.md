# 0001 — One repository with clearly named sections

Date: 6 October 2026. Status: adopted for initial setup.

## Context

The product has a native mobile game and web companion sharing accounts and services. We need easily identifiable sections and coordinated updates without unnecessary operational complexity.

## Decision

Use one repository with `mobile/`, `web/`, `backend/`, `shared/` and `docs/`. Keep existing planning files at the root to preserve references. Follow the selected game engine's standard layout inside `mobile/`. Place the future combat host under `backend/combat-server/`.

## Consequences

Related client/contract/backend changes can be reviewed in one commit or pull request. Each section owns its build and tests. The repository does not require one deployment or a common programming language. Asset storage, engine/framework versions and hosted remote remain separate decisions.

Separate repositories can be introduced later if access, asset scale or independent release ownership warrants them. That change would need explicit contract/version coordination.
