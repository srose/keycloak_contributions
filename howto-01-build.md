# Build Keycloak from source
Required:
- java in version 21

## Command line
Activate build-cache
```bash
export MAVEN_OPTS="-Dmaven.build.cache.enabled=true"
```

```bash
./mvnw clean install
```

```
./mvnw clean install -DskipTests
```

```
./mvnw clean install -Pdistribution
```

```
./mvnw -pl quarkus/deployment,quarkus/dist -am -DskipTests clean install
```

## IDE: IntelliJ
Hint: A build is required to have everything working in IntelliJ

docs/documentation must be added manually: Could be added to [pom.xml in docs](./docs/pom.xml)?

Copy run-configurations from the [launch](./launch/)-directory into current keycloak...

```bash
yes | cp -rf ../launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```

| Config | Description |
|--------|-------------|
|        |             |
|        |             |
