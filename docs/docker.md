# Docker

If you prefer hosting without Docker, see [Self-Hosting](selfhost.md) instead.

Docker Engine and Docker Compose are required.

Note that built images are not provided via Docker Hub.
If you can't or don't want to build them on your server
and don't want to setup a CI/CD system,
[let us know](https://github.com/txtdot/txtdot/issues),
we'll consider setting up a GitHub Actions workflow.

```bash
git clone https://github.com/txtdot/txtdot.git
cd txtdot
docker compose build
docker compose up -d
```
