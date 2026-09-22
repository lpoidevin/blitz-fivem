# Cfx Server

## Server configuration example

```
# Database connection
set mysql_connection_string "mysql://citizenfx@unix(/run/mysqld/mysqld.sock)/fivem"
set mysql_ui false
set mysql_debug false

# Define the server's listening endpoints
endpoint_add_tcp "[::]:30120"
endpoint_add_udp "[::]:30120"

# Define connect endpoints (listing to cfx servers)
set sv_forceIndirectListing true
set sv_listingHostOverride "connect.dev.coursepoursuite.fr"

# Define server endpoints (getEndpoints on client's connection)
set sv_endpoints "endpoint.dev.coursepoursuite.fr"

# Define the file servers configuration
set sv_httpFileServerProxyOnly true
# Add all proxy addresses (connection proxy and cache servers)
set sv_proxyIPRanges "::1/128 2001:41d0:20a:900::629/128 2001:41d0:20a:900::614/128 92.222.230.74/32 92.222.197.110/32"
set adhesive_cdnKey "SomeLongRandomSecretString"
fileserver_add ".*" "https://cache.dev.coursepoursuite.fr"
```

To prevent adding a dev server in the server list

```
sv_master1 ""
```

## Logging configuration of txAdmin

Edit profile config in `txData/<profile>/config.json`.

```json
{
  //...
  "logger": {
    "fxserver": {
      "compress": "gzip", // compress rotated files
      "interval": "1d",
      "maxFiles": 7, //max number of rotated files to keep
      "maxSize": "5G" //max size of rotated files to keep
    },
    "admin": {
      "compress": "gzip",
      "interval": "1d"
      // "maxFiles": 14,
      // "maxSize": "2G"
    },
    "server": {
      "compress": "gzip",
      "interval": "1d",
      "maxFiles": 7,
      "maxSize": "5G"
    }
  }
  //...
}
```

## Developer access

- create user account on the system
- provision default password to change on first use
- deploy SSH key
- add user to `citizenfx` group
- create a symbolic link in home directory to the data directory (`/var/opt/cfx/txData`)

## Deployment access

In case of production server, use chrooted SFTP instead of shared data folder.

SSH key deployment for SFTP access is handle with ansible.

## Notes

On start, FXServer creates:

- `artifact/alpine/opt/cfx-server/server-monitor-token.key`
- `artifact/alpine/opt/cfx-server/server-tls.crt`
- `artifact/alpine/opt/cfx-server/server-tls.key`
- `artifact/alpine/opt/cfx-server/crashes/`
