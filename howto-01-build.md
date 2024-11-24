# Build Keycloak from source
Required:
- java in version 21

## Commandline

Activate build-cache
```bash
export MAVEN_OPTS="-Dmaven.build.cache.enabled=true"
```
Full build including tests
```bash
./mvnw clean install
```

Build without executing tests
```
./mvnw clean install -DskipTests
```
![image](images/build_result_time.png)

Other steps to skip similar to skipTests?


| Switch        | Description                                             |
|---------------|---------------------------------------------------------|
| skipTestsuite | Deactivates the Profile *testsuite*, enabled by default |
| skipExamples  | OpenAPI generation without examples                     |
| ?             | ?                                                       |

Build a distribution with all adapters which are disables by default
```
./mvnw clean install -Pdistribution
```

Build only the quarkus server/distribution part
```
./mvnw -pl quarkus/deployment,quarkus/dist -am -DskipTests clean install
```

## IDE: IntelliJ
**Hint**: A commandline build is required to have everything working in an IDE, see above.
Some maven plugins are generating class files that are necessary.

**Hint**: Use the Build-Menu Item in IntelliJ: *Build Project* but not *Rebuild Project*. 
*Rebuild Project* would remove those class files and you'll end up with compilation errors.

Question: docs/documentation must be added manually in IDE: Could be added to [pom.xml in docs](./docs/pom.xml)?

## Available configurations

| Config                | Description                                                 |
|-----------------------|-------------------------------------------------------------|
| mvn-clean-install-min | mvn running clean install goals and skip unnecessary things |
