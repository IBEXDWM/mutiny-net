# CLAUDE.md

## Project overview

Docker Compose setup for running a Mutinynet signet environment (bitcoind, LND, etc.) locally or in test environments.

## Common commands

```bash
docker-compose up -d                # Start all services
docker-compose down                 # Stop all services
docker-compose logs -f <service>    # Tail logs for a service
docker-compose ps                   # Show running services
docker-compose exec <svc> sh        # Shell into a service
```

## Code conventions

- **File naming**: `docker-compose.yml` for the main compose file; environment-specific overrides as `docker-compose.<env>.yml`
- **Env files**: `.env.sample` checked in; `.env.*` ignored
- **Services**: explicit `image` tags (never `latest` in production configs); pinned versions preferred
- **Volumes**: named volumes for persistent state; bind mounts for config
- **Networks**: explicit network definitions; avoid the default bridge
- **Shell scripts**: `#!/usr/bin/env bash`, `set -euo pipefail` at the top, shellcheck-clean

## Git conventions

- **Commit messages**: conventional commits — `feat(scope): message`, `fix(scope):`, `chore(scope):`
- **Main branch**: `main`

## What to watch out for

- Never commit `.env` files — they contain credentials
- Review image tag changes carefully — implicit updates can break compatibility
- Verify signet/regtest configs are not accidentally pointing to mainnet
- Check `docker-compose.yml` for exposed ports — don't expose internal services publicly
- Volumes are persistent — `docker-compose down -v` wipes state; document this in README
