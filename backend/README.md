# Shared backend

Owns accounts, profiles, clubs/chat, rankings, matchmaking, reward policy, wallet and commerce verification. Begin with clear modules in one product service; split deployments only when evidence warrants it.

An authoritative real-time combat host has a distinct runtime responsibility. Place its engine/server project under `backend/combat-server/` when selected. Keep product APIs separate from per-tick combat transport.

Stack selection is pending. Document service commands, configuration examples without secrets, migrations, validation and recovery here as implemented. Version API contracts under `shared/`. Clients cannot mint coins or choose trusted winners.
