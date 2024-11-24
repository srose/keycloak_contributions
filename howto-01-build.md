# Build Keycloak from source
Required:
- java in version 21
- 

## Commandline

Activate build-cache
```bash
export MAVEN_OPTS="-Dmaven.build.cache.enabled=true"
```
Full build
```bash
./mvnw clean install
```
Build without executing tests
```
./mvnw clean install -DskipTests
```
![image](images/build_result_time.png)

```
./mvnw clean install -Pdistribution
```

```
./mvnw -pl quarkus/deployment,quarkus/dist -am -DskipTests clean install
```

## IDE: IntelliJ
Required:
- Maven

Hint: A commandline build is required to have everything working in IntelliJ, see above.

Use the Build-Menu Item in IntelliJ: Build Project & Rebuild Project.

Question: docs/documentation must be added manually in IDE: Could be added to [pom.xml in docs](./docs/pom.xml)?

```

### Available configurations

| Config                      | Description              |
|-----------------------------|--------------------------|
| mvn-clean-install-skipTests | mvn run as name suggests |
