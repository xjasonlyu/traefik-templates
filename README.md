# traefik-templates

Simple [Traefik v3](https://doc.traefik.io/traefik/) templates with automatic HTTPS from Let's Encrypt.

| Template | Description |
| --- | --- |
| [`docker/`](docker/) | Docker Compose: Traefik + a `whoami` demo service |
| [`swarm/`](swarm/) | Docker Swarm (coming soon) |

## Quick start (Docker Compose)

Requirements:

- A Linux host with Docker and Docker Compose
- Ports 80 and 443 open to the internet
- A DNS record pointing your domain to the host

1. In [`docker/docker-compose.yaml`](docker/docker-compose.yaml), replace `me@mydomain.com` with your email and `whoami.mydomain.com` with your domain.
2. Start it:

   ```bash
   cd docker
   docker compose up -d
   ```

3. Open `https://whoami.<your-domain>`.

## How it works

- Traefik uses host networking, so each service can stay on its own network. Linux only.
- HTTP is redirected to HTTPS, and certificates are stored in the `acme` volume.
- Only containers labeled `traefik.enable=true` are exposed.

## Adding a service

Add labels to any container, in any Compose project:

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.myapp.rule=Host(`myapp.example.com`)"
```

If the container exposes more than one port, also set `traefik.http.services.myapp.loadbalancer.server.port`.

## License

[MIT](LICENSE)
