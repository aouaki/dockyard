# Media Server

A media server configuration to run Plex, Sonarr, Radarr, and Transmission in Docker and behind Traefik.

## What you will get

- [Plex](https://www.plex.tv), your media server – available at plex.your.domain
- [Sonarr](https://sonarr.tv/), a series download manager using torrents – available at sonarr.your.domain
- [Radarr](https://radarr.video/), a movies download manager using torrents – available at radarr.your.domain
- [Transmission](https://transmissionbt.com/), a torrent client that will be used by Sonarr and Radarr  – available at transmission.your.domain
- [Jackett](https://github.com/Jackett/Jackett), a torrent indexer that will provide Sonarr and Radarr with trackers – available at jackett.your.domain
- Plexpy to monitor your plex usage – available at plexpy.your.domain
- [Ombi](https://ombi.io/) to handle your shared Plex users – available at ombi.your.domain


## Installation

### General Setup

- install [Docker](https://www.docker.com/)
- install [Docker Compose](https://docs.docker.com/compose/)
- create a [Plex account](https://www.plex.tv/)
- clone this repository
- clone and setup [the reverse proxy](https://github.com/hkaj/reverse_proxy)
- create a user/group for your media server, you will provide its user id and group id in the `docker-compose up` command.
- create a folder named `media` in this folder (`dockyard`) owned by the `media` user you just created.
- create a `web` docker network with `docker network create web`


### Plex setup
- get your Plex claim token at https://www.plex.tv/claim/. It expires pretty fast, so make sure you claim it just before running next command.
- for initial Plex setup, we will need to connect to the plex server from localhost
- find the local ip used by your plex container by doing `docker inspect plex | grep '"IPAddress":'`. We'll call it `PlexIp`

If you're physically using your server and have access to its graphics interface:
- run `DOMAIN_NAME="..." PLEX_TOKEN="..." USER_ID="..." GROUP_ID="..." docker-compose up -d`
- Using a web browser, go to `PlexIp:32400` and follow the instructions.

If you don't, you will have to bridge the ip address to the server's localhost and your local network to the server's localhost for the port 32400:
- Edit the docker-compose.yaml on the server and replace the lines
```yaml
networks:
  - web
```
with
```yaml
network-mode: host
```
- On your local machine, run ```ssh YourUser@ServerIp -L 8888:127.0.0.1:32400```
- On your local machine, using a web browser, go to `localhost:32400` and follow the instructions


### Transmission

We use [Transmission](https://transmissionbt.com/) as the downloader.

- stop transmission's container
- configure basic auth at `media/transmission/config/settings.json` (you will need to touch `rpc-authentication-required`, `rpc-username` and `rpc-password`)
- start transmission's container


### Sonarr

We use [Sonarr](https://sonarr.tv/) to track and manage TV shows.

- setup auto-update and authentication
- connect transmission as a downloader


### Radarr

We use [Radarr](https://radarr.video/) (a clone of Sonarr) to track and manage movies.

- setup auto-update and authentication
- connect transmission as a downloader


### Jackett

We use [Jackett](https://github.com/Jackett/Jackett) as a proxy between private trackers and our other components.
