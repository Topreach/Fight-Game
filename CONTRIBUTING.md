# Making changes

## Keep changes easy to find

Use the section map in README.md. Place platform-specific code in `mobile/` or `web/`, server logic in `backend/`, and interface definitions in `shared/`. Keep one clear responsibility per module. Add feature subfolders when implementation needs them rather than pre-create dozens of empty folders.

Clients never decide trusted wallet balances or online winners. Shared contracts describe interfaces; they do not require sharing game-engine code with browser UI code.

## Git workflow

Start from `main` and create a short-lived branch such as `feat/mobile-controls`, `fix/web-profile`, or `docs/fighter-research`. Make a focused change, review `git diff`, run the checks appropriate to the affected section, then commit. Use titles such as `feat(mobile): add touch guard controls` or `docs: update fighter evidence`.

Keep unrelated changes separate. Merge reviewed work into `main`. Once a remote exists, use pull requests and branch protection as appropriate. Do not force-push shared history or commit secrets, generated engine files, dependencies or build outputs.

## Updates and upgrades

- Lock dependency and engine versions once selected; commit the applicable lockfiles.
- Document build/run/test commands in each section's README when it becomes runnable.
- For API changes, update `shared/` and the affected clients/server together. Add explicit compatibility handling for breaking changes.
- Use versioned, reviewed migrations for persistent data; document backups and recovery before applying production changes.
- Record significant architecture changes in `docs/decisions/` with context, choice, alternatives and consequences.
- Keep fighter/content/replay versions explicit. Do not assume older saves or replays automatically survive content updates.
- Upgrade one major dependency at a time on a branch and verify affected behavior before merging.

No structure can eliminate future corrections. Small modules, traceable decisions, compatible interfaces and focused validation make corrections manageable.
