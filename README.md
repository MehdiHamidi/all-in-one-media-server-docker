# All-in-One Media Server Docker

A comprehensive Docker Compose setup for running a complete media server stack including automation tools, media servers, and download clients.

## Features

- **Media Servers**
  - Emby (port 8096)
  - Jellyfin (port 8097)
- **Media Management**
  - Sonarr (port 8989) - TV Shows automation
  - Radarr (port 7878) - Movies automation
  - Prowlarr (port 9696) - Indexer management
- **Download Client**
  - qBittorrent (port 8080)
- **Additional Services**
  - FlareSolverr (port 8191) - Bypass Cloudflare protection

## Prerequisites

- Docker
- Docker Compose
- Sufficient storage space for media files
- Basic understanding of Docker and networking

## Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/all-in-one-media-server-docker.git
cd all-in-one-media-server-docker
```

2. Create your environment file:
```bash
cp .env.template .env
```

3. Edit the `.env` file with your specific settings:
```plaintext
TZ="America/New_York"
PUID=1000
PGID=1000
USERDIR=/path/to/Local_Media_Server
```

Note: Replace `/path/to/Local_Media_Server` with your actual media storage path.

4. Create the required directories:
```bash
mkdir -p ${USERDIR}/{config,media}/{sonarr,radarr,prowlarr,emby,jellyfin,qbittorrent}
mkdir -p ${USERDIR}/media/{movies,tv,downloads}
```

5. Start the stack:
```bash
docker-compose up -d
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

After starting the stack, services can be accessed at:

- Sonarr: `http://localhost:8989`
- Radarr: `http://localhost:7878`
- Prowlarr: `http://localhost:9696`
- Emby: `http://localhost:8096`
- Jellyfin: `http://localhost:8097`
- qBittorrent: `http://localhost:8080`
- FlareSolverr: `http://localhost:8191`

## Default Credentials

- qBittorrent:
  - Username: `admin`
  - Password: `adminadmin`

## Configuration

### Permissions

The PUID and PGID in the .env file should match your user's UID and GID. You can find these by running:
```bash
id $USER
```

### Time Zone

Set your correct timezone in the .env file. Find your timezone in the [TZ database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

### Architecture Note

The Emby service includes two image options:
- `emby/embyserver:4.9.0.34` for x86 systems
- `emby/embyserver_arm64v8:4.9.0.34` for ARM systems (e.g., Raspberry Pi)

Uncomment the appropriate line in the docker-compose.yml file for your system architecture.

## Usage

### Basic Commands

Start the stack:
```bash
docker-compose up -d
```

Stop the stack:
```bash
docker-compose down
```

View logs:
```bash
docker-compose logs -f [service_name]
```

Update all containers:
```bash
docker-compose pull
docker-compose up -d
```

## Service Integration

1. Configure Prowlarr first and add your preferred indexers
2. In Prowlarr, add Sonarr and Radarr as applications
3. Configure qBittorrent in both Sonarr and Radarr as your download client
4. Configure your media paths in Sonarr and Radarr
5. Set up your media libraries in Emby and/or Jellyfin

## Maintenance

### Backup

Important directories to backup:
- All directories in the `config` folder
- Database files for each service

### Updates

Check the [LinuxServer.io](https://linuxserver.io/) website for the latest versions of containers and update the image tags in docker-compose.yml accordingly.

## Contributing

Feel free to submit issues and pull requests.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [LinuxServer.io](https://linuxserver.io/) for maintaining most of the containers
- [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) team
- Emby and Jellyfin teams for their media server software