# Hosting Stack Docker Compose Configuration

## Jellyfin Media Server

#### Service Name: jellyfin

**Image**: lscr.io/linuxserver/jellyfin:latest

**Description**: Self-hosted media server, an alternative to Plex.

**Configuration**:
**PUID** and **PGID**: User and group IDs for permissions (optional).

**TZ**: Timezone setting (optional).

**JELLYFIN_PublishedServerUrl**: Public URL for Jellyfin (optional).

**Volumes**: Persistent storage for configuration and media files.

**Devices**: Hardware acceleration for Intel (optional).

**Ports**: `8096` for the web GUI, additional optional ports.

**Restart**: Restart the service unless explicitly stopped.

## Jellyseerr for Jellyfin requests

#### Service Name: jellyseerr

**Image**: fallenbagel/jellyseerr:latest

**Description**: Handles movie and series requests for Jellyfin.

**Configuration**:

**LOG_LEVEL**: Logging level.

**TZ**: Timezone setting (optional).

**Volumes**: Persistent storage for configuration.

**Ports**: `5055` for the web GUI.

**Restart**: Restart the service unless explicitly stopped.

## FreshRSS for self-hosted RSS articles

#### Service Name: freshrss

**Image**: lscr.io/linuxserver/freshrss:latest
**Description**: Self-hosted RSS feed reader.
**Configuration**:
PUID, PGID, TZ: User, group IDs, and timezone.
Volumes: Persistent storage for configuration.
Ports: `8020` for the web GUI.
Restart: Restart the service unless explicitly stopped.

## Full-text RSS extraction for FreshRSS

#### Service Name: fulltextrss

**Image**: heussd/fivefilters-full-text-rss:latest
**Description**: Extracts full-text articles for FreshRSS.
**Ports**: `82` for the web interface.
**Restart**: Restart the service unless explicitly stopped.

## Kavita self-hosted Books

#### Service Name: kavita

**Image**: lscr.io/linuxserver/kavita:latest
**Description**: Self-hosted book server.
**Configuration**:
PUID, PGID, TZ: User, group IDs, and timezone.
Volumes: Persistent storage for configuration and books.
Ports: `5000` for the web interface.
Restart: Restart the service unless explicitly stopped.

## Audiobookshelf for self-hosted Audiobooks

#### Service Name: audiobookshelf

**Image**: ghcr.io/advplyr/audiobookshelf:latest
**Description**: Self-hosted audiobook server.
**Configuration**:
TZ: Timezone setting.
Volumes: Persistent storage for audiobooks, podcasts, and configuration.
Ports: `8090` for the web interface.
Restart: Restart the service unless explicitly stopped.

## Invidious for self-hosted YouTube

#### Service Name: invidious

**Image**: quay.io/invidious/invidious:latest
**Description**: Self-hosted alternative to YouTube.
**Configuration**:
Database settings, domain, and other Invidious configurations.
Healthcheck: Checks the health of the service.
Logging: Configuration for logging.
Depends On: Specifies that it depends on the invidious-db service.

## Database for Invidious

#### Service Name: invidious-db

**Image**: docker.io/library/postgres:14
**Description**: PostgreSQL database for Invidious.
**Volumes**: Persistent storage for database data and initialization script.
**Environment**: Database name, user, and password.
**Healthcheck**: Checks if the database is ready.

## MySQL Database for Nextcloud

#### Service Name: mysqlnextcloud

**Image**: mysql:8.0
**Description**: MySQL database for Nextcloud.
**Volumes**: Persistent storage for database data.
**Environment**: Random root password, database name, user, and password.

## Nextcloud

#### Service Name: nextcloud

**Image**: lscr.io/linuxserver/nextcloud:latest
**Description**: Self-hosted file sharing and collaboration platform.
**Configuration**:
PUID, PGID, TZ: User, group IDs, and timezone.
Volumes: Persistent storage for configuration and data.
Ports: `1443` for the web interface.
Restart: Restart the service unless explicitly stopped.

### Volumes

Volume Name: postgresdata
Description: Defines a Docker volume for PostgreSQL data.

---

# Monitoring Stack Docker Compose Configuration

This Docker Compose configuration sets up a monitoring stack with Grafana, Prometheus, cAdvisor, and Node Exporter. Each service plays a specific role in the monitoring ecosystem.

## Grafana Service

- **Image:** grafana/grafana-oss:latest
- **Container Name:** grafana
- **Ports:** Maps Grafana's internal port 3000 to the host's port 3010 for web access.
- **Environment:**
  - **GF_SERVER_ROOT_URL:** Sets the root URL for Grafana to https://grafana.example.com/.
- **Volumes:** Mounts a local volume (grafana-data) for persistent storage of Grafana data.
- **Restart Policy:** The service restarts unless explicitly stopped.

## Prometheus Service

- **Image:** prom/prometheus:v2.37.9
- **Container Name:** prometheus
- **Ports:** Maps Prometheus's internal port 9090 to the host's port 9090 for web access.
- **Volumes:**
  - **/.prometheusconf:** Mounts local Prometheus configuration files.
  - **prometheus-data:** Mounts a local volume for persistent storage of Prometheus data.
- **Restart Policy:** The service restarts unless explicitly stopped.

## cAdvisor Service

- **Image:** gcr.io/cadvisor/cadvisor:v0.47.0
- **Container Name:** cadvisor
- **Ports:** Maps cAdvisor's internal port 8080 to the host's port 8082 for web access.
- **Volumes:** Mounts various host directories for container metrics collection.
- **Devices:** Grants access to the host kernel message buffer (/dev/kmsg).
- **Privileged:** Runs in privileged mode for enhanced access.
- **Restart Policy:** The service restarts unless explicitly stopped.

## Node Exporter Service

- **Image:** quay.io/prometheus/node-exporter:v1.5.0
- **Container Name:** node_exporter
- **Command:** Specifies a command to set the root filesystem path.
- **PID:** Runs in the host PID namespace for access to process-related metrics.
- **Restart Policy:** The service restarts unless explicitly stopped.
- **Volumes:** Mounts the host root filesystem (/) for metrics collection, with read-only and recursive slave options.

## Volumes

- **grafana-data:**
  - **Driver:** Local driver for Grafana data volume.
- **prometheus-data:**
  - **Driver:** Local driver for Prometheus data volume.

---

# Docker Compose Configuration for Media and Download Services

This Docker Compose file orchestrates various media and download-related services using containerization for easy deployment and management.

## Jellyfin Service

[Jellyfin](https://jellyfin.org/) is a self-hosted media server, serving as an alternative to Plex.

- **Image:** lscr.io/linuxserver/jellyfin:latest
- **Container Name:** jellyfin
- **Environment Variables:**
  - PUID=1000: User ID for file permissions (optional)
  - PGID=1000: Group ID for file permissions (optional)
  - TZ=America/Los_Angeles: Timezone configuration (optional)
  - JELLYFIN_PublishedServerUrl=jellyfin.example.com: Public server URL (optional)
- **Volumes:**
  - /.jellyfin:/config: Persistent storage for Jellyfin configuration
  - /ssd2/TV:/data/tvshows: TV shows library path (optional)
  - /ssd2/Books:/data/books: Books library path (optional)
  - /ssd2/Movies:/data/movies: Movies library path (optional)
- **Devices:**
  - /dev/dri/:/dev/dri/: For Intel HW Acceleration
- **Ports:**
  - 8096:8096: Web GUI
  - 8920:8920: Optional port
  - 7359:7359/udp: Optional port
  - 1900:1900/udp: Optional port
- **Restart Policy:** unless-stopped

## Jellyseerr Service

[Jellyseerr](https://github.com/fallenbagel/Jellyseerr) is used for making movie and series requests for Jellyfin.

- **Image:** fallenbagel/jellyseerr:latest
- **Container Name:** jellyseerr
- **Environment Variables:**
  - LOG_LEVEL=debug: Logging level
  - TZ=America/Los_Angeles: Timezone configuration (optional)
- **Ports:**
  - 5055:5055: Web GUI
- **Volumes:**
  - /.jellyseerr:/app/config: Persistent storage for Jellyseerr configuration
- **Restart Policy:** unless-stopped

## FreshRSS Service

[FreshRSS](https://freshrss.org/) is a self-hosted RSS reader.

- **Image:** lscr.io/linuxserver/freshrss:latest
- **Container Name:** freshrss
- **Environment Variables:**
  - PUID=1000: User ID for file permissions
  - PGID=1000: Group ID for file permissions
  - TZ=America/Los_Angeles: Timezone configuration
- **Volumes:**
  - /.freshrss:/config: Persistent storage for FreshRSS configuration
- **Ports:**
  - 8020:80: Web GUI
- **Restart Policy:** unless-stopped

## FullTextRSS Service

[FiveFilters Full-Text RSS](https://fivefilters.org/content-only/) is used to extract full-text articles for FreshRSS.

- **Image:** heussd/fivefilters-full-text-rss:latest
- **Container Name:** fulltextrss
- **Ports:**
  - 82:80
- **Restart Policy:** unless-stopped

## Kavita Service

[Kavita](https://github.com/gcoda/kavita) is a self-hosted eBook server.

- **Image:** lscr.io/linuxserver/kavita:latest
- **Container Name:** kavita
- **Environment Variables:**
  - PUID=1000: User ID for file permissions
  - PGID=1000: Group ID for file permissions
  - TZ=America/Los_Angeles: Timezone configuration
- **Volumes:**
  - /.kavita:/config: Persistent storage for Kavita configuration
  - /ssd2/Books:/data: Books library path
- **Ports:**
  - 5000:5000
- **Restart Policy:** unless-stopped

## Audiobookshelf Service

[Audiobookshelf](https://github.com/advplyr/audiobookshelf) is a self-hosted Audiobook server.

- **Container Name:** audiobookshelf
- **Image:** ghcr.io/advplyr/audiobookshelf:latest
- **Ports:**
  - 8090:80
- **Environment Variables:**
  - TZ=America/Los_Angeles: Timezone configuration
- **Volumes:**
  - /ssd2/Audiobooks:/audiobooks: Audiobooks library path
  - /ssd2/podcasts:/podcasts: Podcasts path
  - /.audiobookshelf:/config: Persistent storage for Audiobookshelf configuration
  - /.absmetadata:/metadata: Persistent storage for metadata
- **Restart Policy:** unless-stopped

## Invidious Service

[Invidious](https://github.com/iv-org/invidious) is a self-hosted alternative to YouTube.

- **Container Name:** invidious
- **Image:** quay.io/invidious/invidious:latest
- **Restart Policy:** unless-stopped
- **Ports:**
  - 3040:3000: Invidious web UI
- **Environment Variables:**
  - Configuration details for database connection, domain, host binding, etc.
- **Healthcheck:**
  - Test to check the availability of Invidious every 30 seconds
- **Logging:**
  - Configured to rotate logs with a maximum size of 1GB and 4 files
- **Dependencies:**
  - invidious-db: Database for Invidious

## Invidious Database Service

This service provides the PostgreSQL database for the Invidious service.

- **Container Name:** invidious-db
- **Image:** docker.io/library/postgres:14
- **Restart Policy:** unless-stopped
- **Volumes:**
  - postgresdata:/var/lib/postgresql/data: Persistent storage for PostgreSQL data
  - ./config/sql:/config/sql: SQL initialization scripts
  - ./docker/init-invidious-db.sh:/docker-entrypoint-initdb.d/init-invidious-db.sh: Initialization script
- **Environment Variables:**
  - POSTGRES_DB: invidious
  - POSTGRES_USER: invidious
  - POSTGRES_PASSWORD: invidious@1
- **Healthcheck:**
  - Check if the PostgreSQL database is ready

## MySQL for Nextcloud Service

MySQL service for Nextcloud, a self-hosted cloud storage solution.

- **Restart Policy:** unless-stopped
- **Image:** mysql:8.0
- **Hostname:** mysqlnextcloud
- **Container Name:** mysqlnextcloud
- **Volumes:**
  - /.mysqlnextcloud:/var/lib/mysql: Persistent storage for MySQL data
- **Environment Variables:**
  - MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
  - MYSQL_DATABASE: nextcloud
  - MYSQL_USER: nextcloud
  - MYSQL_PASSWORD: NextCloud11@

## Nextcloud Service

[Nextcloud](https://nextcloud.com/) is a self-hosted cloud storage solution.

- **Image:** lscr.io/linuxserver/nextcloud:latest
- **Container Name:** nextcloud
- **Environment Variables:**
  - PUID=1000: User ID for file permissions
  - PGID=1000: Group ID for file permissions
  - TZ=America/Los_Angeles: Timezone configuration
- **Volumes:**
  - /.nextcloud:/config: Persistent storage for Nextcloud configuration
  - /ssd2/nextcloud:/data: Nextcloud data directory
- **Ports:**
  - 1443:443: Nextcloud web UI port
- **Restart Policy:** unless-stopped

## Grafana Service

[Grafana](https://grafana.com/) is an open-source analytics and monitoring platform.

- **Image:** grafana/grafana-oss:latest
- **Container Name:** grafana
- **Ports:**
  - 3010:3000: Grafana web UI port
- **Environment Variables:**
  - GF_SERVER_ROOT_URL=https://grafana.example.com/: Root URL for Grafana
- **Volumes:**
  - grafana-data:/var/lib/grafana: Persistent storage for Grafana data
- **Restart Policy:** unless-stopped

## Prometheus Service

[Prometheus](https://prometheus.io/) is an open-source monitoring and alerting toolkit.

- **Image:** prom/prometheus:v2.37.9
- **Container Name:** prometheus
- **Ports:**
  - 9090:9090: Prometheus web UI port
- **Volumes:**
  - /.prometheusconf:/etc/prometheus: Prometheus configuration directory
  - prometheus-data:/prometheus: Persistent storage for Prometheus data
- **Restart Policy:** unless-stopped

## cAdvisor Service

[cAdvisor](https://github.com/google/cadvisor) provides container metrics.

- **Image:** gcr.io/cadvisor/cadvisor:v0.47.0
- **Container Name:** cadvisor
- **Ports:**
  - 8082:8080: cAdvisor web UI port
- **Volumes:**
  - /:/rootfs:ro
  - /var/run:/var/run:ro
  - /sys:/sys:ro
  - /var/lib/docker/:/var/lib/docker:ro
  - /dev/disk/:/dev/disk:ro
- **Devices:**
  - /dev/kmsg
- **Privileged:** true
- **Restart Policy:** unless-stopped

## Node Exporter Service

[Node Exporter](https://github.com/prometheus/node_exporter) exports machine metrics.

- **Image:** quay.io/prometheus/node-exporter:v1.5.0
- **Container Name:** node_exporter
- **Command:** --path.rootfs=/host
- **PID:** host
- **Restart Policy:** unless-stopped
- **Volumes:**
  - /:/host:ro,rslave

## Volumes

### postgresdata

- **Driver:** local

### grafana-data, prometheus-data

- **Driver:** local

---

# Docker Compose Configuration for Various Services

## Dashy Service

- **Image:** lissy93/dashy
- **Container Name:** dashy
- **Ports:**
  - 8888:80 # Mapping host port 8888 to container port 80
- **Volumes:**
  - /.dashy/conf.yml:/app/public/conf.yml # Mounting configuration file
  - /.dashy/icons:/app/public/item-icons # Mounting icons directory
- **Restart Policy:** unless-stopped

## Glances Service

- **Image:** nicolargo/glances:latest-full
- **Container Name:** glances
- **Ports:**
  - 61208:61208 # Glances web UI port
  - 61209:61209 # Glances streaming port
- **Volumes:**
  - /var/run/docker.sock:/var/run/docker.sock # Allowing access to Docker socket
- **Environment Variables:**
  - GLANCES_OPT=-w # Glances options
  - PUID=1000 # User ID for file permissions
  - PGID=1000 # Group ID for file permissions
  - TZ=America/Los_Angeles # Timezone configuration
- **Restart Policy:** unless-stopped

## Uptime-Kuma Service

- **Image:** louislam/uptime-kuma:1
- **Container Name:** uptime-kuma
- **Ports:**
  - 3001:3001 # Uptime-Kuma web UI port
- **Volumes:**
  - /.kuma:/app/data # Mounting data directory
- **Restart Policy:** unless-stopped

## Flaresolverr Service

- **Image:** ghcr.io/flaresolverr/flaresolverr:latest
- **Container Name:** flaresolverr
- **Environment Variables:**
  - TZ=America/Los_Angeles # Timezone configuration
- **Ports:**
  - 8191:8191 # Flaresolverr port
- **Restart Policy:** unless-stopped

## AdGuard Home Service

- **Image:** adguard/adguardhome:latest
- **Container Name:** adguardhome
- **Volumes:**
  - /.adguardwork:/opt/adguardhome/work # AdGuard Home work directory
  - /.adguardconf:/opt/adguardhome/conf # AdGuard Home configuration directory
- **Ports:**
  - 53:53/tcp # DNS port
  - 53:53/udp # DNS port (UDP)
  - 86:80/tcp # AdGuard Home web UI port
  - 3000:3000/tcp # AdGuard Home API port
- **Restart Policy:** unless-stopped

## Your_Spotify Service

- **Image:** lscr.io/linuxserver/your_spotify:latest
- **Container Name:** your_spotify
- **Environment Variables:**
  - PUID=1000 # User ID for file permissions
  - PGID=1000 # Group ID for file permissions
  - TZ=America/Los_Angeles # Timezone configuration
  - APP_URL=https://spotify.example.com # Spotify app URL
  - SPOTIFY_PUBLIC=2acbb5d42e094deeb2d44bf98a38deb6 # Spotify public key
  - SPOTIFY_SECRET=7b0ebc5fa0ea4a3fb4a19d29aaa17e1e # Spotify secret key
  - CORS=all # Enable Cross-Origin Resource Sharing
  - MONGO_ENDPOINT=mongodb://mongo:27017/your_spotify # MongoDB endpoint
- **Ports:**
  - 85:80 # Mapping host port 85 to container port 80
  - 445:443 # Mapping host port 445 to container port 443
- **Restart Policy:** unless-stopped

## MongoDB Service

- **Container Name:** mongo
- **Image:** mongo
- **Restart Policy:** always # Restart policy
- **Volumes:**
  - /.your_spotify_db:/data/db # MongoDB data directory

## MySQL Service

- **Container Name:** mysql
- **Restart Policy:** unless-stopped # Restart policy
- **Image:** mysql:8.0
- **Hostname:** mysql
- **Volumes:**
  - /.mysqlsema:/var/lib/mysql # MySQL data directory
- **Environment Variables:**
  - MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
  - MYSQL_DATABASE: semaphore
  - MYSQL_USER: semaphore
  - MYSQL_PASSWORD: semaphore

## Semaphore Service

- **Container Name:** semaphore
- **Restart Policy:** unless-stopped # Restart policy
- **Ports:**
  - 3030:3000 # Semaphore web UI port
- **Image:** ghcr.io/imagegenius/semaphore:latest
- **Environment Variables:**
  - TZ: America/Los_Angeles # Timezone configuration
  - ANSIBLE_HOST_KEY_CHECKING: False
  - SEMAPHORE_DB_USER: semaphore
  - SEMAPHORE_DB_PASS: semaphore
  - SEMAPHORE_DB_HOST: mysql # MySQL database host
  - SEMAPHORE_DB_PORT: 3306 # MySQL database port
  - SEMAPHORE_DB_DIALECT: mysql # Database dialect
  - SEMAPHORE_DB: semaphore
  - SEMAPHORE_PLAYBOOK_PATH: /tmp/semaphore/
  - SEMAPHORE_ADMIN_PASSWORD: Admin@123
  - SEMAPHORE_ADMIN_NAME: admin
  - SEMAPHORE_ADMIN_EMAIL: admin@example.com
  - SEMAPHORE_ADMIN: admin
  - ACCESS_KEY_ENCRYPTION: gs72mPntFATGJs9qK0pQ0rKtfidlexiMjYCH9gWKhTU= # Access key encryption
- **Depends On:**
  - mysql # Dependency on MySQL service

## Volumes

### uptime-kuma

- **Type:** Named Volume
