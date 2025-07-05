# Media Setup

Media setup using docker compose to run Radarr, Sonarr, Jackett, RDT Client and Plex.

Caddy reverse proxies each service at a subpath, allowing Mac users to access the services at
`http://${hostname}.local` from any device on their local network.

Radarr: `http://${hostname}.local/movies`
Sonarr: `http://${hostname}.local/tv`
RDT Client: `http://${hostname}.local/downloads`
Jackett: `http://${hostname}.local/jackett`

Plex Server needs to be added to your Plex account and will be available on any clients signed
in to that account.

## Setup

Create an `.env` file with the following configuration:

```bash
# Path to download directory for RDT Client, ideally same volume as Media
DOWNLOADS="/Volumes/Media/Downloads"
# Path to base media directory
MEDIA="/Volumes/Media"
# Plex claim token, get it from https://plex.tv/claim (you need a plex account obviously)
PLEX_CLAIM_TOKEN="claim-xxxxxxxxxxxxxxxxxx"
```

## Running

Run the services using Docker Compose, I highly reccommend OrbStack on macOS.

```bash
docker compose up -d
```

The services will run in the background and automatically restart on system boot as long as
Docker is configured to start on boot.

Stop the services with:

```bash
docker compose down
```

## Notes

- Real Debrid is a paid service, get an API key here: https://real-debrid.com and add it at
  `http://${hostname}.local/downloads`
- Jackett turns torrent sites into RSS feeds that Sonarr and Radarr can query.
- Adding content to Sonarr and Radarr will search using Jackett and the add downloads to RDT Client.
- Once RDT Client completes, Sonarr and Radarr will move and rename the files to
  `${MEDIA}/TV Shows` and `${MEDIA}/Movies` respectively.
- You can configure Plex to automatically rescan the library when new content is added.
