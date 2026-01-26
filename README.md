## 👋 Welcome to borg 🚀

Deduplicating backup program with compression and encryption

## 📋 Description

Deduplicating backup program with compression and encryption

## 🚀 Services

- **app**: borgwarehouse/borgwarehouse:latest
- **notify**: caronc/apprise:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/borg/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/borg" ~/.local/srv/docker/borg
cd ~/.local/srv/docker/borg
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install borg
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_API_TOKEN=changeme_cronjob_key_min_32_chars
APP_SECRET_KEY=changeme_nextauth_secret_min_32_chars
EMAIL_SERVER_PORT=587
EMAIL_SERVER_HOST=172.17.0.1
EMAIL_SERVER_MAIL_FROM=no-reply@${BASE_DOMAIN_NAME:-${BASE_HOST_NAME
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:59072

## 📂 Volumes

- `./rootfs/config/ssh` - Data storage
- `./rootfs/config/borg` - Data storage
- `./rootfs/data/borg` - Data storage
- `./rootfs/data/log/borg` - Data storage

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
