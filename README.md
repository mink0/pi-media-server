# pi-media-server

Lightweight media server inteneded to be used on low powered hardware (**Raspberry Pi** and such) based on Docker.
Basically it is all you need to turn on your **Raspberry Pi 4** into download'n'streaming box in 2020.

### Features

- Easy install
- Configuration will be stored on external device to prevent data loss in case sd card is dead
- **miniDLNA**: lightweight DLNA server which provides direct streaming and strict folder view
- **Transmission**: it was created to be light & fast on slow hardware
- **TOR** proxy (http, socks5): to bypass ISP blocking lists
- **Jackett**: well-known tracker indexer with preconfigured TOR proxy
- **Radarr**: for serching movies
- **Sonarr**: for searching tv shows
- **Lidarr**: for searching music
- **Nginx**: for easy access using custom dmain names

### Media directory tree (will be created automatically on `$MEDIA_DIR`):

    /downloads  - used as default for Transmission
    /config     - all working configuration is stored here
      /jackett
      /radarr
      /sonarr
      ...
    /media      - all downloaded media are hardlinked or stored there
      /movies
      /tvshows
      /music
      ...

## Installation

- Clone this repo

      git clone https://github.com/mink0/pi-media-server.git 

- Set path to `$MEDIA_DIR` in `.env`:

      cp env.sample .env
      nano .env

- Install `docker` and `docker-compose`:

      sudo apt-get install -y docker docker-compose

- Launch services in daemon mode:

      docker-compose up -d

Thats it! 

Check from your TV for new local media server available (like `raspberry:minidlna`).
Got to `http://<server-ip>` to access available services and setup Jackett + [Rad|Lid|Son]arr to use your prefered trackers.

If you add DNS records for `.*server.domain` -> `server-ip` all services will also be accessible using subdomains:
  
    http(s)://{{service-name}}.{{server.domain}} 

or by an alias, for example:

- http://tv.media.local
- http://movie.media.local
- http://music.media.local
- http://jackett.media.local

## Add Jackett to Radarr, Sonarr, Lidarr

Use the following link to add aggregated Jackett indexer results:
    
    settings/indexers -> Add indexer -> Torznab (custom) -> URL
    http://jackett:9117/api/v2.0/indexers/all/results/torznab

or add each tracker indexers individually.

## Additioanl config

To avoid long-running errors add this to `/etc/sysctl.conf` on your server:

```
net.core.rmem_max = 4194304
net.core.wmem_max = 1048576
fs.inotify.max_user_watches=524288
```

and then apply it:

    sudo sysctl -p

## Optional services

- Plex
- Emby
- Gerbera
- Readymedia

To use them, uncomment related sections in [docker-compose.yml](./docker-compose.yml)

### Plex

Technically Plex could be run in parallel with miniDLNA. I will recommend to disable server-side transcoding and enable DLNA from Plex UI.
You will need provide proper `PLEX_CLAIM` on first launch.

## TODO

- Add Jacket indexers auto-import
- Systemd scripts
