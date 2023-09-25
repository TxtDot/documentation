# Docker

If you prefer hosting without Docker, see [Self-Hosting](selfhost.md) instead.

Download docker-compose.yml and txtdot configs,
edit them and then start the container:
```bash
wget https://raw.githubusercontent.com/TxtDot/txtdot/main/docker-compose.yml
wget -O .env https://raw.githubusercontent.com/TxtDot/txtdot/main/.env.example
nano .env
docker compose up -d
```

Alternatively, you can configure txtdot with
the `environment` section of docker-compose config
(don't forget to remove .env and `volumes`).
