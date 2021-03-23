# Keycloak Postgresql Docker Container

Docker container information [here](https://hub.docker.com/r/centos/postgresql-10-centos7)

## Docker Configuration

Create a `/database/.env` file and populate it with appropriate settings.

```conf
POSTGRESQL_DATABASE={database name}
POSTGRESQL_USER={username}
POSTGRESQL_PASSWORD={password}
```

| Key                 | Value    | Description                                                                                             |
| ------------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| POSTGRESQL_DATABASE | {string} | The name of Keycloak database. Must be the same as the above **DB_DATABASE**                            |
| POSTGRESQL_USER     | {string} | The name of the default database user administrator. Must be the same as the above **DB_USER**          |
| POSTGRESQL_PASSWORD | {string} | The password for the default database user administrator. Must be the same as the above **DB_PASSWORD** |

## Docker Commands

```bash
docker build -t keycloak-db .
docker run --env-file=../.env -d keycloak-db
```
