# Build Keycloak

TODO
- Need to double it here?
- When to use what and why?
- Quarkus server is wrapping keycloak?
- UIs are bundled as jars?
- ...

## Command line

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
Hint: One build line run is required to have local binaries?

Just have it built/indexed/compiled AFTER a build on command line?

(all the things IntelliJ can help you with?)

docs/documentation must be added additionally

Could be added to [docs](./docs/pom.xml)?
