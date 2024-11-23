# Run Keycloak from source
Required: 
- java in version 21
- Completed [build](howto-01-build.md) 

## Commandline

```
java -jar quarkus/server/target/lib/quarkus-run.jar start-dev --bootstrap-admin-password=admin --bootstrap-admin-username=admin
```

```
cd quarkus/server
mvn clean quarkus:dev -Dquarkus.args="start-dev --bootstrap-admin-password=admin --bootstrap-admin-username=admin"
```

## IDE (IntelliJ)

Copy run-configurations from the [launch](./launch/)-directory into current keycloak...

```bash
yes | cp -rf ../launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```

| Config | Description |
|--------|-------------|
|        |             |
|        |             |
