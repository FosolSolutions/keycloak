# Keycloak Docker Container

Docker container information [here](https://hub.docker.com/r/jboss/keycloak/)

## Docker Configuration

Create a `/app/.env` file and populate it with appropriate settings.

```conf
# Keycloak configuration
PROXY_ADDRESS_FORWARDING=true
KEYCLOAK_USER={username}
KEYCLOAK_PASSWORD={password}
KEYCLOAK_IMPORT=/tmp/realm-export.json -Dkeycloak.profile.feature.scripts=enabled -Dkeycloak.profile.feature.upload_scripts=enabled
KEYCLOAK_LOGLEVEL=WARN
ROOT_LOGLEVEL=WARN

# Database configuration
# These are optional if you don't want to run a separate database for keycloak.
DB_VENDOR=POSTGRES
DB_ADDR={docker service name}
DB_DATABASE={database name}
DB_USER={username}
DB_PASSWORD={password}
```

| Key                      | Value                                                                   | Description                                                                                                                    |
| ------------------------ | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| PROXY_ADDRESS_FORWARDING | [true\|false]                                                           | Informs Keycloak to handle proxy forwarded requests correctly.                                                                 |
| KEYCLOAK_USER            | {string}                                                                | The name of the Keycloak Realm administrator.                                                                                  |
| KEYCLOAK_PASSWORD        | {string}                                                                | The password for the Keycloak Realm administrator.                                                                             |
| KEYCLOAK_IMPORT          | /tmp/real-export.json -Dkeycloak.profile.feature.upload_scripts=enabled | The path to the configuration file to initialize Keycloak with. This also includes an override to enable uploading the script. |
| KEYCLOAK_LOGLEVEL        | [WARN\|ERROR\|INFO]                                                     | The logging level for Keycloak.                                                                                                |
| ROOT_LOGLEVEL            | [WARN\|ERROR\|INFO]                                                     | The logging level for the root user of the container.                                                                          |
| DB_VENDOR                | [POSTGRES\|...]                                                         | The database that Keycloak will use.                                                                                           |
| DB_ADDR                  | {docker service name}                                                   | The host name of the Keycloak DB found in the `docker-compose.yaml`                                                            |
| DB_DATABASE              | {string}                                                                | Name of the Keycloak database.                                                                                                 |
| DB_USER                  | {string}                                                                | The name of the default database user administrator.                                                                           |
| DB_PASSWORD              | {string}                                                                | The password for the default database user administrator.                                                                      |

```bash
docker build -t keycloak .
docker run --env-file=../.env -d keycloak
```
