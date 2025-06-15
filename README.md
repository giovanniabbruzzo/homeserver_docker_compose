# Media Server Stack with VPN Protection

A Docker Compose configuration for a comprehensive media server stack with built-in VPN protection. This setup includes Jellyfin for media streaming, Gluetun for VPN connectivity, and various media management tools - all working together seamlessly.

## 🚀 Services

- **Portainer**: Container management UI (port 9000)
- **Jellyfin**: Media streaming server (port 8096)
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
   - Bender: `http://localhost:8080`
   - Portainer: `http://localhost:9000`
   - qBittorrent: `http://localhost:8085`
   - Jackett: `http://localhost:9117`
   - Sonarr: `http://localhost:8989`
   - Prowlarr: `http://localhost:9696`
   - JellyFin: ``http://localhost:8096`
   - NextCloud: `http://localhost:8081`

3. Stop the stack:
```bash
docker-compose down
```

## ⚙️ Configuration

### Bender
- Simple UI to add links to each container, kind of like a launcher

### Jellyfin
- Configure media libraries through the web interface
- Default media path: `/media/data/jellyfin/media`

### Gluetun (VPN)
- VPN Provider: NordVPN
- Protocol: WireGuard
- Update frequency: 24h

To get the wireguard key for Gluten VPN with NordVPN is possible to follow this YouTube tutorial https://www.youtube.com/watch?v=eY4sYGYt0b4. 
In short, using Linux:
   - Connect to NordVPN client with your credentials.
   - Connect to NordVPN website on the browser and make sure to be in NordVPN page. From https://my.nordaccount.com/dashboard/ click on NordVPN.
   - At the bottom you will find "Get Access Token".
   - Now click on "Generate new token" and copy the token.
   - You can use that token with this command to get the Wireguard token: ```curl -s -u token:<ACCESS_TOKEN> https://api.nordvpn.com/v1/users/services/credentials | jq -r .nordlynx_private_key```
   - You may need to get the info regarding you VPN server connection with this command: ```curl -s "https://api.nordvpn.com/v1/servers/recommendations?&filters\[servers_technologies\]\[identifier\]=wireguard_udp&limit=1" | jq -r '.[]|.hostname, .station, (.locations|.[]|.country|.city.name), (.locations|.[]|.country|.name), (.technologies|.[].metadata|.[].value), .load'```
   - Finally, you can add these info in a .env file in the same folder as docker-compose.yml: 
      VPN_SERVICE_PROVIDER=nordvpn
      WIREGUARD_PRIVATE_KEY=<YOUR_WIREGUARD_ACCESS_TOKEN>
      WIREGUARD_ADDRESSES=<SERVER_ADDRESS>/<NUMBER_FOUND_AT_BOTTOM_OF_RESPONSE>

### Nextcloud
- Access URL: `http://localhost:8081`
- Default data directory: `/media/data/nextcloud/data`
- Features:
  - File synchronization
  - Online document editing with Collabora
  - Redis caching for improved performance
  - MariaDB for reliable data storage

### Collabora Online
- Access through Nextcloud
- Supports document editing
- Secure SSL connection
- Custom domain configuration required

### Redis Cache
- Improves Nextcloud performance
- Persistent storage in ./redis
- No direct configuration needed

### Database (MariaDB)
- Automated setup with environment variables
- Data persistence in ./nextclouddb
- Secure random root password generation

### Nginx Proxy Manager (Optional)
- Web UI at port 81
- SSL certificate management
- Reverse proxy configuration
- Custom domain support

### qBittorrent
- Web UI port: 8085
- Default credentials: admin/adminadmin
- Download directory: `/downloads`

### Filebrowser
- Web UI port: 9999
- Default credentials: admin/admin

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
├── secrets/
│   └── secrets.env
├── qbittorrent/
├── jackett/
├── sonarr/
├── prowlarr/
├── nextclouddb/
├── redis/
├── media/
│   ├── jellyfin/
│   └── nextcloud/
│       ├── html
│       ├── custom_apps
│       ├── config
│       └── data
└── nginx-proxy/
    ├── data
    └── letsencrypt
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