# Bitwarden (self-hosted)

Bitwarden is **not** managed by a hand-written compose file in this repo. It is
installed and updated with the official self-hosting script.

- Management script: `/home/justin/bitwarden.sh`
- Data directory:    `/home/justin/bwdata/`
- Generated compose: `/home/justin/bwdata/docker/docker-compose.yml`
  (auto-generated — header warns "Do not make changes to this file")
- Secrets live in:   `/home/justin/bwdata/env/*.env` (never commit)

The web vault is exposed on `localhost:8081` and reverse-proxied by Caddy at
`https://${CADDY_DOMAIN}` (see `../caddy/`).

## Common operations

```bash
./bitwarden.sh start      # start the stack
./bitwarden.sh stop
./bitwarden.sh restart
./bitwarden.sh updateself # update the script
./bitwarden.sh update     # pull new images and regenerate compose
```

The generated compose and the `env/` secrets are intentionally gitignored.
