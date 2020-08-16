# pi-media-server

Easy install and lightweight media server inteneded to be used on low powered hardware (raspberry pi and such).

### Features

- TOR proxy (http, socks5): preconfigured by deafult for Jackett (to bypass blocking list)
- miniDLNA: lightweight and fast DLNA server which provides direct streaming
- Jackett: for torrent seraching
- Radarr: for movies
- Sonarr: for tv shows
- Lidarr: for music

### Media directory tree (will be creted automatically on $MEDIA_DIR):

    /config
      /jackett
      /radarr
      /sonarr
      ...
    /media
      /movies
      /tvshows
      ...
    /downloads

## Installation

- Clone this repo

- Edit variables in `.env`:

    cp env.sample .env
    nano .env

- Install `docker` and `docker-compose`:

    sudo apt-get install -y docker docker-compose

- Launch server as a daemon:

    docker-compose up -d

Thats it! 

Got to `http://<server-ip>` to access all available services and setup Jackett + [Rad|Lid|Son]arr to use your preffered torrent trackers.

If you add DNS records for `.*server.domain` -> `server-ip` all services will also be accessible using subdomains:
  
    http(s)://{{service-name}}.{{server.domain}} 

or by an alias, for example:

- http://tv.media.local
- http://movies.media.local
- http://music.media.local
- http://jackett.media.local

## Add Jackett to Radarr, Sonarr, etc.

Use the following link to add aggregated Jackett indexer results:
    
    settings/indexers -> Add indexer -> Torznab (custom) -> URL
    http://jackett:9117/api/v2.0/indexers/all/results/torznab

or add each torrent tracker individually.

## Additioanl config

To avoid errors add this to /etc/sysctl.conf on your server:

```
net.core.rmem_max = 4194304
net.core.wmem_max = 1048576
fs.inotify.max_user_watches=524288
```

and then apply it:

    sudo sysctl -p

### Optional services

To use them, uncomment related sections in `docker-compose.yml`:

- Plex
- Emby
- Gerbera
- Readymedia

## TOOD

- Add Jacket indexers auto import

