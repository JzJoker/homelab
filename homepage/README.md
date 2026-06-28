# Homepage

Dashboard config for the [Homepage](https://gethomepage.dev) service in the
**core** stack. Bind-mounted into the container at `/app/config` from
`/opt/homepage/config` on the host.

## Secrets

`services.yaml` references secrets via `{{HOMEPAGE_VAR_*}}` placeholders, mapped
to environment variables in `secrets.yaml`. The real values come from the
`HOST_IP`, `CADDY_DOMAIN`, `PIHOLE_API_KEY`, `TAILSCALE_*` and `PORTAINER_API_KEY`
entries in the core stack's `.env` (see `../core/.env.example`).

**Never commit real keys here.** The committed `services.yaml` is sanitized;
if the live `/opt/homepage/config/services.yaml` still has literal keys inline,
switch it to the same `{{HOMEPAGE_VAR_*}}` placeholders so the two stay in sync
and no secret can leak.

## Not committed

- `logs/` — runtime logs (gitignored)
- `*.backup` — stray editor/backup files (gitignored)
