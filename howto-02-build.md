# Build Keycloak from source
Required:
- java in version 21

## Command-line
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
| skipTests     | Prevents test-execution                                 |
| skipTestsuite | Deactivates the Profile *testsuite*, enabled by default |
| skipExamples  | OpenAPI generation without examples                     |
| ?             | ?                                                       |

Favorite build command
```bash
./mvnw clean install -DskipTests -DskipTestsuite -DskipExamples
```

Build a distribution with all adapters using the distribution-profile
```
./mvnw clean install -Pdistribution
```

Other profiles?

### Performance
Activate build-cache for performance
```bash
export MAVEN_OPTS="-Dmaven.build.cache.enabled=true"
```

### Partial builds

Build only the quarkus part
```
./mvnw -pl quarkus/deployment,quarkus/dist -am -DskipTests clean install
```

Question: What other partial builds are useful to speed up roundtrips?

## IDE: IntelliJ
**Hint**: A command-line build is recommended to have everything working in an IDE, see above.
Some maven plugins are generating class files that are necessary.

**Hint**: Use the Build-Menu Item in IntelliJ: *Build Project* but not *Rebuild Project*. 
*Rebuild Project* would remove those class files and you'll end up with compilation errors.

Question: docs/documentation must be added manually in IDE: Could be added to [pom.xml in docs](./docs/pom.xml)?

### Available run-configurations

| Config                | Description                                                 |
|-----------------------|-------------------------------------------------------------|
| mvn-clean-install-min | mvn running clean install goals and skip unnecessary things |

Question: What useful build-configs or templates should be shared?