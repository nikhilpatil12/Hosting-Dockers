# Self-Hosted Docker Services

Welcome to the self-hosted Docker services repository! This collection of Docker Compose configurations enables you to deploy and manage various self-hosted services for media, downloads, and monitoring. Below is an overview of each service, its purpose, and the functionality it provides.

---

## Media and Download Services

### 1. Radarr

- **Description:** Movie management service.
- **Purpose:** Organize and manage movie libraries.

### 2. Sonarr

- **Description:** TV series management service.
- **Purpose:** Organize and manage TV series libraries.

### 3. Prowlarr

- **Description:** Movie and TV series indexer.
- **Purpose:** Indexes movies and TV series for use in Radarr and Sonarr.

### 4. Bazarr

- **Description:** Subtitle management service.
- **Purpose:** Fetch and manage subtitles for movies and TV series.

### 5. Readarr

- **Description:** eBook management service.
- **Purpose:** Organize and manage eBook libraries.

### 6. Speakarr

- **Description:** Audiobook management service.
- **Purpose:** Organize and manage audiobook libraries.

### 7. qBittorrent

- **Description:** Torrent downloader service.
- **Purpose:** Download files via BitTorrent.

### 8. NZBGet

- **Description:** Usenet downloader service.
- **Purpose:** Download files via Usenet.

### 9. Transmission

- **Description:** Torrent downloader service.
- **Purpose:** Lightweight torrent downloader.

### 10. Deluge

- **Description:** Torrent downloader service.
- **Purpose:** Feature-rich torrent downloader.

---

## Media Server

### 1. Jellyfin

- **Description:** Self-hosted media server.
- **Purpose:** Stream media content to various devices.

### 2. Jellyseerr

- **Description:** Handles movie and series requests for Jellyfin.
- **Purpose:** Allows users to request specific movies and TV series.

### 3. FreshRSS

- **Description:** Self-hosted RSS feed reader.
- **Purpose:** Stay updated on your favorite websites.

### 4. Full-text RSS extraction

- **Description:** Extracts full-text articles for FreshRSS.
- **Purpose:** Provides full-text content for RSS articles.

### 5. Kavita

- **Description:** Self-hosted book server.
- **Purpose:** Manage and read eBooks.

### 6. Audiobookshelf

- **Description:** Self-hosted audiobook server.
- **Purpose:** Manage and listen to audiobooks.

### 7. Invidious

- **Description:** Self-hosted alternative to YouTube.
- **Purpose:** Watch YouTube videos without ads.

### 8. Database for Invidious

- **Description:** PostgreSQL database for Invidious.
- **Purpose:** Stores data for the Invidious service.

### 9. Nextcloud

- **Description:** Self-hosted file sharing and collaboration platform.
- **Purpose:** Store, share, and collaborate on files.

### 10. MySQL Database for Nextcloud

- **Description:** MySQL database for Nextcloud.
- **Purpose:** Stores data for the Nextcloud service.

---

## Monitoring Stack

### 1. Grafana

- **Description:** Analytics and monitoring platform.
- **Purpose:** Visualize and analyze data from various sources.

### 2. Prometheus

- **Description:** Monitoring and alerting toolkit.
- **Purpose:** Collect and store metrics from services, used with Grafana.

### 3. Node Exporter

- **Description:** Exports machine metrics.
- **Purpose:** Collect system-level metrics, used with Grafana.

### 4. cAdvisor

- **Description:** Analyzes resource usage and performance characteristics.
- **Purpose:** Collects and exports container metrics, used with Grafana.

---

## Additional Services

### 1. Dashy

- **Description:** Real-time dashboard service.
- **Purpose:** Display information from various sources in a dashboard.

### 2. Glances

- **Description:** System monitoring service.
- **Purpose:** Monitor system resources and performance.

### 3. Uptime-Kuma

- **Description:** Uptime monitoring service.
- **Purpose:** Monitor the uptime of websites and services.

### 4. Flaresolverr

- **Description:** Captcha solving service.
- **Purpose:** Solve captchas for various applications.

### 5. AdGuard Home

- **Description:** DNS and ad-blocking service.
- **Purpose:** Block ads and protect against online threats.

### 6. Your_Spotify

- **Description:** Self-hosted Spotify server.
- **Purpose:** Host and manage your Spotify instance.

### 7. MongoDB

- **Description:** NoSQL database service.
- **Purpose:** Store data for various applications.

### 8. MySQL

- **Description:** Relational database service.
- **Purpose:** Store data for various applications.

### 9. Semaphore

- **Description:** Continuous integration and deployment service.
- **Purpose:** Automate the testing and deployment of code.
