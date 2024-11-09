# Deploy Gitea

Part of the [*Komodo Hub* collection.](https://github.com/komodo-hub/komodo-hub)

Deploys [Gitea](https://about.gitea.com/) with Postgres database.

https://docs.gitea.com/installation/install-with-docker#postgresql-database

## Komodo Resource TOML

```toml
[[stack]]
name = "gitea"
[stack.config]
repo = "komodo-hub/deploy-gitea"
file_paths = [
  "compose.yaml",
  # "caddy.compose.yaml" # Deploy https proxy
]
environment = """
  ## Container options
  GITEA_TAG = 1
  RESTART = unless-stopped
  LOGGING_DRIVER = local
  USER_UID = 1000
  USER_GID = 1000

  ## Configure custom ports
  HTTP_PORT = 3000
  SSH_PORT = 222

  ## Volumes - Optional - Defaults using docker volumes
  # GITEA_DATA_PATH = /path/to/gitea/data
  # DB_DATA_PATH = /path/to/db/data

  ## Configure Postgres connection
  POSTGRES_PASSWORD = [[GITEA_POSTGRES_PASSWORD]]
  POSTGRES_USER = gitea
  POSTGRES_DB = gitea

  ## Required for Caddy deploy
  GITEA_DOMAIN = gitea.example.com
  CADDY_TAG = latest
"""

[[variable]]
name = "GITEA_POSTGRES_PASSWORD"
value = "your_postgres_password"
is_secret = true
```

