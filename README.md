# Media Server

A media server configuration to run Plex, Sonarr, Radarr, and Transmission in Docker and behind Traefik.


## First run

- install [Docker](https://www.docker.com/)
- install [Docker Compose](https://docs.docker.com/compose/)
- create a [Plex accout](https://www.plex.tv/)
- clone this repository
- clone and setup [the reverse proxy](https://github.com/hkaj/reverse_proxy)
- create a user for your media server, export its `$USER_ID` and `$GROUP_ID`.
- create a folder named `media` in this folder (`dockyard`) owned by $USER_ID:$GROUP_ID from your `media` user.
- get your Plex claim token at https://www.plex.tv/claim/
- create a `web` docker network with `docker network create web`
- run `DOMAIN_NAME="..." PLEX_TOKEN="..." USER_ID="$USER_ID" GROUP_ID="$GROUP_ID" docker-compose up -d`
- profit :)

## Config


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
