# Keycloak

Open Source Identity and Access Management for Modern Applications and Services.

Add authentication to applications and secure services with minimum fuss. No need to deal with storing users or authenticating users. It's all available out of the box.

You'll even get advanced features such as User Federation, Identity Brokering and Social Login.

- [Keycloak](https://www.keycloak.org/)
- [Docker](https://hub.docker.com/r/jboss/keycloak/)
- [Github](https://github.com/keycloak/keycloak-containers/tree/master/server)

## Docker Configuration

To configure Keycloak following these steps.

- [Setup Keycloak](./app/README.md)
- [Setup Database](./database/README.md) _This step is optional based on your configuration_

## Export Realm Configuration

After configuring Keycloak you can export the configuration to a JSON file so that it can be used to initialize a new Keycloak instance.
If you use the UI to export it will not contain all the necessary information and settings, thus the need for this CLI option.

More information [here](https://www.keycloak.org/docs/latest/server_admin/index.html#_export_import).

Once the keycloak container is running, ssh into it and execute the following commands;

### Export the Configuration

```bash
# Determine your container name
docker ps
# SSH into the container
docker exec -it {container name} bash
# Export the realm information to the configured volume
/opt/jboss/keycloak/bin/standalone.sh \
  -Dkeycloak.migration.action=export \
  -Dkeycloak.migration.provider=singleFile \
  -Dkeycloak.migration.realmName=example \
  -Dkeycloak.migration.file=/tmp/realm-export.json \
  -Dkeycloak.migration.usersExportStrategy=REALM_FILE \
  -Dkeycloak.migration.strategy=OVERWRITE_EXISTING \
  -Djboss.http.port=8888 \
  -Djboss.https.port=9999 \
  -Djboss.management.http.port=7777
```

## Import Realm

If you update your `/app/.env` configuration file with the following it will import the realm on startup.

```conf
KEYCLOAK_IMPORT=/tmp/realm-export.json -Dkeycloak.profile.feature.scripts=enabled -Dkeycloak.profile.
```

> However if you start the keycloak server without importing, check if it is up and running with only the **master** realm and then manually do the import below, it works.

To import a previously exported realm configuration execute the following command;

```bash
# Determine your container name
docker ps
# SSH into the container
docker exec -it {container name} bash
# Import the realm from the configured volume
/opt/jboss/keycloak/bin/standalone.sh \
  -Djboss.socket.binding.port-offset=100 \
  -Dkeycloak.migration.action=import \
  -Dkeycloak.profile.feature.scripts=enabled \
  -Dkeycloak.profile.feature.upload_scripts=enabled \
  -Dkeycloak.migration.provider=singleFile \
  -Dkeycloak.migration.file=/tmp/realm-export.json
```

or

```bash
docker run -e KEYCLOAK_USER=<USERNAME> -e KEYCLOAK_PASSWORD=<PASSWORD> \
    -e KEYCLOAK_IMPORT=/tmp/example-realm.json -v /tmp/example-realm.json:/tmp/example-realm.json jboss/keycloak
```
