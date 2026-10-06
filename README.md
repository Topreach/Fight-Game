# Fight Game

A realistic native mobile boxing game and a responsive web companion, using shared backend services.

## Find the section you need

| Folder | Responsibility |
|---|---|
| [mobile](mobile/README.md) | Native game, combat, animation, arena, cameras and touch controls |
| [web](web/README.md) | Companion website: profiles, clubs, rankings, lessons and replay videos |
| [backend](backend/README.md) | Accounts, matchmaking, verified results, wallet, rewards and community |
| [shared](shared/README.md) | API/data contracts shared across clients and services |
| [docs](docs/README.md) | Planning, architecture decisions and contribution guidance |

One Git repository keeps related changes reviewable together. These folders are boundaries, not separate deployed services. The game engine and web/backend frameworks have not been selected; this structure contains documentation, not runnable applications.

## Project documents

- [Product plan](fight_game.md)
- [Fighter research](fighter_research.md)
- [Implementation readiness](implementation_readiness.md)
- [Platform structure](platform_structure.md)
- [How to make changes](CONTRIBUTING.md)

## First development milestones

1. Choose mobile test devices and evaluate the engine.
2. Prove touch controls, movement, contact and replay in `mobile/`.
3. Build the integrated live-fight slice.
4. Add web and backend features through versioned shared contracts.

Existing planning files stay at the repository root to preserve their links. New detailed documentation belongs under `docs/`.
