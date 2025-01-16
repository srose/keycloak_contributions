# Run Keycloak from source
Required:
- Completed [build](howto-02-build.md)

All [keycloak application parameters](https://www.keycloak.org/server/all-config) can be used.

## Command-line
Required:
- java in version 21

Run from maven goal via quarkus:dev
```
cd quarkus/server
../../mvnw clean quarkus:dev -Dquarkus.args="start-dev --verbose"
```

To run from jar-file pom.xml of /quarkus/server remove:
```
<!-- Necessary for proper execution of IDELauncher -->
<!-- Can be removed as part of the https://github.com/keycloak/keycloak/issues/22455 enhancement -->
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-vertx-http-dev-ui-resources</artifactId>
    <scope>provided</scope>
</dependency>
<!-- this dependency is necessary to start the IDELauncher -->
<dependency>
    <groupId>com.oracle.database.jdbc</groupId>
    <artifactId>ojdbc11</artifactId>
    <scope>provided</scope>
</dependency>
```
```
java -jar quarkus/server/target/lib/quarkus-run.jar start-dev --verbose
```

## IDE (IntelliJ)
Required:
- Working IDE (IntelliJ) using java

### Available run-configurations

| Config                         | Description                                                                                      |
|--------------------------------|--------------------------------------------------------------------------------------------------|
| keycloak-ju5-in-memory         | Uses class Keycloak from junit5 to run keycloak with in memory db                                |
| keycloak-ju5-in-memory-logging | Uses class Keycloak from junit5 to run keycloak with in memory db with extensive logging enabled |
| keycloak-ju5-postgres          | Uses class from junit5 to run keycloak with postgres db (requires external service)        |
| mvn-quarkus-dev-debug          | Uses maven and quarkus:dev task to run keycloak with debugging enabled on port 5005              |
| debug-remote-5005              | Remote debugging on port 5005                                                                    |

Question: What useful launch-configs or templates should be shared?

### keycloak-quarkus-server-app 
IntelliJ recognizes a quarkus app, but its not possible to launch or use it.

Question: Ideas or is the Keycloak in keycloak-junit5 the only answer?