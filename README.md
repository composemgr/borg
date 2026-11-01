## 👋 Welcome to borg 🚀

BorgWarehouse web UI for managing deduplicated Borg backups

## 📋 Description

BorgWarehouse is a web interface for creating and managing BorgBackup
repositories. It exposes the web UI on the host docker bridge and an SSH
endpoint for Borg clients. Repositories, configuration, SSH keys and the host
SSH key directory are bind-mounted under `./volumes`.

## 🚀 Services

- **app**: borgwarehouse/borgwarehouse:latest - web UI and Borg SSH server

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/borg/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/borg" ~/.local/srv/docker/borg
cd ~/.local/srv/docker/borg
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install borg
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_HOST_NAME=borg.example.com
BORG_APP_URL=https://borg.example.com
APP_RUN_AS=1000
APP_API_TOKEN=replace-with-a-random-32-char-key
APP_SECRET_KEY=replace-with-a-random-secret-key
EMAIL_SERVER_PORT=587
EMAIL_SERVER_HOST=172.17.0.1
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, web UI on `172.17.0.1:64124`, SSH on host port `64901` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the `app` service carries the proxy labels and joins the external
network. The proxy variants publish no host ports; the SSH service remains
reachable on the internal container network for Borg clients that use the
same network or an explicit TCP proxy.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64124
- **Borg SSH**: ssh://{host}:64901

Complete the first-run setup in the web UI, then create repositories and
users there. The app's `APP_RUN_AS` UID/GID must own the mounted directories.

## 📂 Volumes

- `./volumes/config/borg` - BorgWarehouse application configuration
- `./volumes/config/ssh` - Authorized client SSH keys
- `./volumes/config/ssh_host` - SSH host keys
- `./volumes/data/borg` - Borg repositories

## 🔐 Security

- Set unique `APP_SECRET_KEY` and `APP_API_TOKEN` values before deployment
- `APP_RUN_AS` must be non-root and own all mounted directories
- Protect the SSH host key and repository directories with regular backups
- Use HTTPS via the reverse proxy in production
- Regularly update Docker images for security patches
- Do not expose Borg SSH publicly without firewall rules and key-only access

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- A non-root host UID/GID for the mounted data directories

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
