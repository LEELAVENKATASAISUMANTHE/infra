# infra

Docker Compose configurations for self-hosted services, managed via Dokploy.

## Services

| Service | Directory | Description |
|---------|-----------|-------------|
| Redpanda | `redpanda/` | Kafka-compatible streaming platform + Console UI |
| ClamAV | `clamav/` | Antivirus daemon with automatic virus DB updates |

## How to deploy in Dokploy

1. Push this repo to GitHub / GitLab / Gitea.
2. In Dokploy, create a new **Docker Compose** application for each service.
3. Set the **Repository** to this repo's clone URL.
4. Set the **Compose File Path** for each app:
   - Redpanda → `redpanda/docker-compose.yml`
   - ClamAV → `clamav/docker-compose.yml`
5. In Dokploy's **Environment** tab, paste the contents of the corresponding `.env.example`, fill in real values, and save.
6. Deploy.

## Notes

- **Redpanda**: Set `REDPANDA_EXTERNAL_HOST` to your server's public IP or domain — external Kafka clients use this to connect.
- **ClamAV**: Expects ~600–900 MB RAM. First start is slow (freshclam downloads the full virus DB, ~250 MB). Subsequent restarts are fast due to the persistent `clamav_db` volume.
- Named volumes (`redpanda_data`, `clamav_db`) survive redeployments in Dokploy by default.
- Redpanda Console runs on port 8080 — firewall it or put it behind Dokploy's Traefik proxy with auth.
