# home-media server

## Add Jackett to Radarr, Sonarr, etc.

Use the following link to add aggregated Jackett indexer results:
    
    settings/indexers -> Add indexer -> Torznab (custom) -> URL
    http://jackett:9117/api/v2.0/indexers/all/results/torznab

## Additioanl config

### Add this to /etc/sysctl.conf

```
net.core.rmem_max = 4194304
net.core.wmem_max = 1048576
echo fs.inotify.max_user_watches=524288
```

And then apply it:

    sudo sysctl -p



## TOOD
