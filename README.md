# Media Server Stack with VPN Protection

A Docker Compose configuration for a comprehensive media server stack with built-in VPN protection. This setup includes Jellyfin for media streaming, Gluetun for VPN connectivity, and various media management tools - all working together seamlessly.

## 🚀 Services

- **Portainer**: Container management UI (port 9000)
- **Jellyfin**: Media streaming server
- **Gluetun**: VPN client (NordVPN)
- **qBittorrent**: Torrent client (port 8085)
- **Media Management**:
  - Jackett: Torrent proxy (port 9117)
  - Sonarr: TV series automation (port 8989)
  - Prowlarr: Indexer manager (port 9696)

## 📋 Prerequisites

- Docker Engine (20.10+)
- Docker Compose (3.9+)
- NordVPN subscription
- Minimum 2GB RAM
- Sufficient storage space for media files

## 🛠️ Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/homeserver_docker_composer.git
cd homeserver_docker_composer
```

2. Create required directories:
```bash
mkdir -p /media/data/jellyfin/{config,cache,media}
mkdir -p /home/sunnyhouse_server/docker/{qbittorrent,jackett,sonarr,prowlarr}/data
```

3. Create environment file:
```bash
cat << EOF > .env
TZ=Your/Timezone
PUID=1000
PGID=1000
DOCKERDIR=/home/sunnyhouse_server/docker
EOF
```

4. Configure VPN credentials:
   - Get your WireGuard credentials from NordVPN
   - Update the `WIREGUARD_PRIVATE_KEY` in the docker-compose.yml

## 🚀 Usage

1. Start the stack:
```bash
docker-compose up -d
```

2. Access the services:
   - Portainer: `http://localhost:9000`
   - qBittorrent: `http://localhost:8085`
   - Jackett: `http://localhost:9117`
   - Sonarr: `http://localhost:8989`
   - Prowlarr: `http://localhost:9696`

3. Stop the stack:
```bash
docker-compose down
```

## ⚙️ Configuration

### Jellyfin
- Configure media libraries through the web interface
- Default media path: `/media/data/jellyfin/media`

### Gluetun (VPN)
- VPN Provider: NordVPN
- Protocol: WireGuard
- Update frequency: 24h

### qBittorrent
- Web UI port: 8085
- Default credentials: admin/adminadmin
- Download directory: `/downloads`

## 🔒 Security Notes

1. All sensitive services are routed through VPN
2. Container privileges are restricted
3. Docker socket is read-only
4. Automatic container updates enabled

## 📁 Directory Structure

```plaintext
/home/sunnyhouse_server/docker/
├── docker-compose.yml
├── .env
├── qbittorrent/
├── jackett/
├── sonarr/
├── prowlarr/
└── media/
    └── jellyfin/
```

## 🔧 Maintenance

- Check logs:
```bash
docker-compose logs -f [service_name]
```

- Update containers:
```bash
docker-compose pull
docker-compose up -d
```

- View container status:
```bash
docker-compose ps
```

## 🚨 Troubleshooting

1. VPN Connection Issues:
   - Verify NordVPN credentials
   - Check Gluetun logs: `docker-compose logs gluetun`

2. Media Server Access:
   - Ensure correct PUID/PGID settings
   - Verify directory permissions

3. Container Crashes:
   - Check logs: `docker-compose logs [service_name]`
   - Verify available system resources

## 📝 License

MIT License

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request