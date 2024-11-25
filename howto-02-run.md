# Run Keycloak from source
Required:
- Completed [build](howto-01-build.md)

All [keycloak application parameters](https://www.keycloak.org/server/all-config) can be used.

## Command-line
Required:
- java in version 21

Run from maven goal via quarkus:dev
```
cd quarkus/server
../../mvnw clean quarkus:dev -Dquarkus.args="start-dev --verbose"
```

To run from jar-file pom.xml of /quarkus/server needs an addition:
```
<dependency>
    <groupId>com.oracle.database.jdbc</groupId>
    <artifactId>ojdbc8</artifactId>
</dependency>
```
```
java -jar quarkus/server/target/lib/quarkus-run.jar start-dev --verbose
```

## IDE (IntelliJ)
Required:
- Working IDE (IntelliJ) using java

### Available run-configurations

| Config                     | Description                                                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------|
| launcher-in-memory         | Uses IDELauncher from keycloak to run keycloak with in memory db                                |
| launcher-in-memory-logging | Uses IDELauncher from keycloak to run keycloak with in memory db with extensive logging enabled |
| mvn-quarkus-dev-debug      | Uses maven and quarkus:dev task to run keycloak with debugging enabled on port 5005             |
| debug-remote-5005          | Remote debugging on port 5005                                                                   |

Question: What useful launch-configs or templates should be shared?

### keycloak-quarkus-server-app 
IntelliJ recognizes a quarkus app, but its not possible to launch or use it.

Question: Ideas or is the IDELauncher the only answer?