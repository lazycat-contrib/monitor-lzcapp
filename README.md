# monitor for LazyCat

- Requested upstream https://github.com/monitor-probe/monitor, version 1.5.1.
- Docker documentation https://monitor-document.pages.dev/install/docker.
- Package `community.lazycat.app.monitor`, Linux amd64, minimum LazyCat OS 1.6.0.
- Requested image `docker.io/stqfdyr/monitor:1.5.1`, delivered via `docker.1ms.run` with required source/mirror digest verification.
- Native scratch image entrypoint is `/monitor-hub --db /data/monitor.db`. Additional command arguments set the listener, themes directory and HTTPS site; do not prepend another executable or shell.
- Uses service `run_as: 65534:65534` for the upstream UID/GID and persistent directory ownership. `/lzcapp/var/data` maps to `/data`. Timezone Asia/Shanghai.

## Authentication and agents

Native random emergency-password authentication is retained. Get the first password privately from the application startup log; this package neither invents an unsupported password environment variable nor writes a preset credential to the database. Three-stage login injection captures only POST `/api/auth/login`, persists only HTTP 200 with `ok: true`, then fills the password-only input on `/admin` routes. Automatic submission is disabled to prevent stale-password lockouts. After resetting/changing the native password, log in successfully once to update the saved password. The capture hooks require LazyCat authentication. GitHub OAuth is optional and unconfigured.

The dashboard and administration remain behind LazyCat login. Only `/api/agent/`, `/agent/` and `/install.sh` bypass that outer login, following upstream reverse-proxy instructions. Native registration keys and per-node tokens remain mandatory. There is no public `/` or `/api/` wildcard. No host port, Docker socket or host-system directory is exposed.

This package deploys the hub only. Install the official agent separately on machines you own/control. They must be able to resolve and reach the LazyCat application HTTPS hostname; browser-based LazyCat login is not a machine tunnel. It does not automatically enroll or access real machines. Public unauthenticated status-page sharing is not enabled by this package.

## Publication and verification

Build with the reusable LazyCat workflow and versioned Release assets. Both automatic store publishers are disabled; submit only the independently verified Release URL and SHA256 to MiaoMiao community store, never the official store.

Per user instruction, no file-chooser interceptor, smoke job or runtime gate is added. Configuration/JavaScript checks, workflow lint, image digest matching and LPK verification are not proof of device startup, password autofill, node registration or WebSocket reporting. These runtime behaviors remain unverified.
