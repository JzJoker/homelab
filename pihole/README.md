# Pi-hole

Pi-hole runs as part of the **core** stack — see
[`../core/docker-compose.yaml`](../core/docker-compose.yaml).

It is the DNS server for all Tailscale-connected devices and provides
network-wide ad/tracker blocking.

- Web UI: `http://${HOST_IP}:8082`
- DNS:    `${HOST_IP}:53` (tcp/udp)
- Config persists in the external named volumes `pihole_data` and `dnsmasq_data`
  (not stored as files in this repo).

Detailed Pi-hole lists/config are tracked in the separate `pihole` repo.
