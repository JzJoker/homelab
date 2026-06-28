# homelab

Docker stacks and configs for the services running on **brickserver**, my home
server (Ubuntu 24.04, Intel i7-8700). All inter-device traffic and external
access routes through Tailscale; Pi-hole is the network-wide DNS/ad blocker and
Caddy provides automatic HTTPS via Tailscale certs.

## Layout

| Path | What |
|---|---|
| `core/` | Main stack — Homepage, Portainer, Pi-hole, Netdata, Speedtest Tracker, OpenClaw. Deployed from `/opt/compose`. |
| `caddy/` | Caddy reverse proxy (`network_mode: host`, Tailscale socket for HTTPS). Deployed from `/home/justin/caddy-docker`. |
| `bitwarden/` | Notes only — Bitwarden is managed by `bitwarden.sh` (generated compose + secrets are not committed). |
| `pihole/` | Notes — Pi-hole runs in the core stack; config lives in named volumes. |

## Networks & volumes (created out-of-band)

The core stack references pre-existing external Docker resources:

```bash
docker network create homelab
# openclaw_default is created by the OpenClaw stack
docker volume create pihole_data
docker volume create dnsmasq_data
docker volume create portainer_data
```

## Usage

```bash
cd core
cp .env.example .env     # then fill in real secrets
docker compose up -d

cd ../caddy
docker compose up -d
```

## Secrets

Real secrets live in `.env` files that are **gitignored**. Each stack ships a
`.env.example` template — copy it to `.env` and fill in values locally. Never
commit `.env`, `bitwarden/env/`, or the generated Bitwarden compose.

## Services (ports bound to the Tailscale IP `${HOST_IP}`)

| Service | Port | URL |
|---|---|---|
| Homepage | 3000 | http://${HOST_IP}:3000 |
| Portainer | 9443 | https://${HOST_IP}:9443 |
| Pi-hole | 8082 / 53 | http://${HOST_IP}:8082 |
| Netdata | 19999 | http://${HOST_IP}:19999 |
| Speedtest Tracker | 8080 / 8443 | http://${HOST_IP}:8080 |
| Bitwarden (via Caddy) | 8081 | https://${CADDY_DOMAIN} |
