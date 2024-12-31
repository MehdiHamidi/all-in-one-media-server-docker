# All-in-One Media Server Docker (Latest Versions)

⚠️ **WARNING: LATEST BRANCH STABILITY NOTICE** ⚠️

This branch uses the latest versions of all containers (`latest` tags). While this provides access to the newest features, it may lead to:
- Unexpected behavior
- Service interruptions
- Compatibility issues
- Database corruption
- Broken functionality after updates

**For a stable production environment, use the main branch main

## Features

- **Media Servers**
  - Emby (latest) - Modern media server
  - Jellyfin (latest) - Open-source media server
- **Media Management**
  - Sonarr (latest) - TV Shows automation
  - Radarr (latest) - Movies automation
  - Prowlarr (latest) - Indexer management
- **Download Client**
  - qBittorrent (latest)
- **Additional Services**
  - FlareSolverr (latest) - Cloudflare bypass solution

## Version Information

All services use the `latest` tag from their respective repositories:
```yaml
sonarr:
    image: linuxserver/sonarr:latest
radarr:
    image: linuxserver/radarr:latest
prowlarr:
    image: linuxserver/prowlarr:latest
flaresolverr:
    image: ghcr.io/flaresolverr/flaresolverr:latest
emby:
    image: emby/embyserver:latest  # or emby/embyserver_arm64v8:latest for ARM
jellyfin:
    image: linuxserver/jellyfin:latest
qbittorrent:
    image: linuxserver/qbittorrent:latest
```

## Prerequisites

- Docker (latest version recommended)
- Docker Compose V2
- Sufficient storage space for media files
- Basic understanding of Docker and networking
- Backup solution in place (strongly recommended)

## Installation

1. Clone the repository and switch to the latest branch:
```bash
git clone https://github.com/yourusername/all-in-one-media-server-docker.git
cd all-in-one-media-server-docker
git checkout latest
```

2. Create your environment file:
```bash
cp .env.template .env
```

3. Edit the `.env` file with your settings:
```plaintext
TZ="America/New_York"
PUID=1000
PGID=1000
USERDIR=/path/to/Local_Media_Server
```

4. Create the required directories:
```bash
mkdir -p ${USERDIR}/{config,media}/{sonarr,radarr,prowlarr,emby,jellyfin,qbittorrent}
mkdir -p ${USERDIR}/media/{movies,tv,downloads}
```

5. Start the stack:
```bash
docker compose up -d
```

## Directory Structure

```
Local_Media_Server/
├── config/
│   ├── sonarr/
│   ├── radarr/
│   ├── prowlarr/
│   ├── emby/
│   ├── jellyfin/
│   └── qbittorrent/
└── media/
    ├── movies/
    ├── tv/
    └── downloads/
```

## Service Access

Services are available at:
- Sonarr: `http://localhost:8989`
- Radarr: `http://localhost:7878`
- Prowlarr: `http://localhost:9696`
- Emby: `http://localhost:8096`
- Jellyfin: `http://localhost:8097`
- qBittorrent: `http://localhost:8080`
- FlareSolverr: `http://localhost:8191`

## Update Management

### Automatic Updates Warning ⚠️

If you enable automatic updates in any of the services, be aware that this may cause version mismatches and stability issues. Consider these guidelines:

1. Always check the GitHub releases/changelog before updating
2. Update one service at a time
3. Monitor logs after updates
4. Have a backup ready
5. Consider setting up monitoring for service availability

### Manual Update Process

```bash
# Pull new images
docker compose pull

# Update services one at a time (recommended)
docker compose up -d sonarr
# Wait and verify functionality
docker compose up -d radarr
# And so on...

# Or update all at once (risky)
docker compose up -d
```

## Rollback Process

If you encounter issues after an update:

1. Stop the problematic service:
```bash
docker compose stop [service_name]
```

2. Remove the container:
```bash
docker compose rm [service_name]
```

3. Pull a specific older version:
```bash
docker pull linuxserver/[service_name]:previous_version
```

4. Update docker-compose.yml with the specific version
5. Restart the service:
```bash
docker compose up -d [service_name]
```

## Monitoring and Maintenance

### Log Monitoring
```bash
# Watch logs for stability issues
docker compose logs -f [service_name]
```

### Health Checks

Monitor container health:
```bash
docker compose ps
docker stats
```

### Backup Requirements

Critical to maintain backups when running latest versions:
- All configuration directories
- Database files
- Docker Compose and environment files

## Troubleshooting Latest Versions

Common issues and solutions:

1. Container fails to start after update:
   - Check logs: `docker compose logs [service_name]`
   - Verify compatibility with other services
   - Consider rolling back to previous version

2. Database errors:
   - Stop the container
   - Restore from backup
   - Check for known issues with the latest version

3. API compatibility issues:
   - Ensure all interconnected services are compatible
   - Check GitHub issues for known problems
   - Consider downgrading affected services

## Contributing

- Report bugs and version compatibility issues
- Share successful/unsuccessful version combinations
- Submit improvements to documentation

## Support and Resources

- [Sonarr GitHub](https://github.com/Sonarr/Sonarr)
- [Radarr GitHub](https://github.com/Radarr/Radarr)
- [Prowlarr GitHub](https://github.com/Prowlarr/Prowlarr)
- [LinuxServer.io](https://linuxserver.io/)
- [FlareSolverr GitHub](https://github.com/FlareSolverr/FlareSolverr)
- [Emby](https://emby.media/)
- [Jellyfin](https://jellyfin.org/)
