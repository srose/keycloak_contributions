# Run Keycloak from source

Hot-reload?
Switch to a file in keycloak_srose IDE to have run-configs working

Just run...
- see admin-ui/account-ui
- No database
- Use endpoint via http-client -> see logs, debug entry point, ...

## Commandline

```
java -jar quarkus/server/target/lib/quarkus-run.jar start-dev --bootstrap-admin-password=admin --bootstrap-admin-username=admin
```

```
cd quarkus/server
mvn clean quarkus:dev -Dquarkus.args="start-dev --bootstrap-admin-password=admin --bootstrap-admin-username=admin"
```

## IDE (IntelliJ)

Run-Configurations in [launch](./launch)-directory

```
cp ../launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```

## Steering which local dev-mode UIs to use...

TODO