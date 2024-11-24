# Run Keycloak from source
Required:
- 
- Completed [build](howto-01-build.md)

## Commandline
Required:
- java in version 21
- Maven


```
java -jar quarkus/server/target/lib/quarkus-run.jar start-dev --bootstrap-admin-password=admin --bootstrap-admin-username=admin
```

```
cd quarkus/server
mvn clean quarkus:dev -Dquarkus.args="start-dev --bootstrap-admin-password=admin --bootstrap-admin-username=admin"
```

## IDE (IntelliJ)
Required:
- Working IDE using java, mvn, etc.

Run configurations for different purposes:

| Config                     | Description                                                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------|
| debug-remote-5005          | Remote debugging on port 5005                                                                   |
| launcher-in-memory         | Uses IDELauncher from keycloak to run keycloak with in memory db                                |
| launcher-in-memory-debug   | Uses IDELauncher from keycloak to run keycloak with in memory db with debugging enabled         |
| launcher-in-memory-logging | Uses IDELauncher from keycloak to run keycloak with in memory db with extensive logging enabled |
| mvn-quarkus-dev-debug      | Uses maven and quarkus:dev task to run keycloak with debugging enabled                          |
|                            |                                                                                                 |
