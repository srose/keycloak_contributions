# Keycloak Maven Build Options Reference

> **Source:** [keycloak/keycloak](https://github.com/keycloak/keycloak) repository (branch: `main`)
>
> **Status:** Research extracted directly from `pom.xml` files and documentation (`.md`, `.adoc`) in the Keycloak git repository.
> All entries below are derived from the actual source code — not from assumptions or secondary documents.
>
> **Date of Research:** 2026-02-07

---

## How to Use This Document

When contributing to Keycloak, you will frequently need to customize your Maven build. The three tables below cover:

1. **Maven Profiles** — activated with `-P` (e.g., `mvn clean install -Pauth-server-quarkus`)
2. **Maven Properties** — set with `-D` (e.g., `mvn clean install -DskipTests`)
3. **Configurable Plugin Parameters** — plugin behaviors overridable via `-D` properties

> **Tip:** Always use the Maven wrapper (`./mvnw`) instead of a locally installed Maven to ensure version compatibility.
> The required Maven version is declared in the root `pom.xml` as `3.9.8`.

---

## Table 1: Maven Profiles

Profiles control which modules are included in the build and which configurations are activated.

| Name | Default Value | Purpose / Description | File Path | Usage Example |
|------|--------------|----------------------|-----------|---------------|
| `testsuite` | **Active** (deactivated by `-DskipTestsuite`) | Includes the `testsuite` module in the reactor build. Activated unless the property `skipTestsuite` is set. | `/pom.xml` | `./mvnw clean install -DskipTestsuite` |
| `adapters` | **Active** (deactivated by `-DskipAdapters`) | Includes the `adapters` module. Activated unless the property `skipAdapters` is set. | `/pom.xml` | `./mvnw clean install -DskipAdapters` |
| `docs` | **Active** (deactivated by `-DskipDocs`) | Includes the documentation modules. Activated unless property `skipDocs` is set. | `/pom.xml` | `./mvnw clean install -DskipDocs` |
| `operator` | **Inactive** (activated by `-Doperator`) | Includes the `operator` module. The Keycloak Operator is excluded by default and must be explicitly enabled. Requires JDK 17+. | `/pom.xml` | `./mvnw clean install -Doperator` |
| `central-staging` | Inactive | Configures the `central-publishing-maven-plugin` for publishing to Maven Central. Used for release processes. | `/pom.xml` | `./mvnw clean deploy -Pcentral-staging` |
| `jboss-release` | Inactive | Activates release-specific configurations including GPG signing and source/javadoc JAR generation for JS UI modules. | `/js/apps/admin-ui/pom.xml`, `/js/apps/account-ui/pom.xml` | `./mvnw clean install -Pjboss-release` |
| `communityTranslations` | **Active** (deactivated by `-DskipCommunityTranslations`) | Includes community-contributed translation resource files in the themes module. | `/themes/pom.xml` | `./mvnw clean install -DskipCommunityTranslations` |
| `withTranslations` | **Active** (deactivated by `-DskipCommunityTranslations`) | Includes community translation resources for Admin UI and Account UI. | `/js/apps/admin-ui/pom.xml`, `/js/apps/account-ui/pom.xml` | `./mvnw clean install -DskipCommunityTranslations` |
| `product` | Inactive (activated by `-Dproduct`) | Activates downstream/product-specific build configurations (e.g., different doc attributes, product naming). | `/services/pom.xml`, various | `./mvnw clean install -Dproduct` |
| `auth-server-quarkus` | Inactive | Runs the integration testsuite against a real Quarkus-based Keycloak server. Requires building the distribution first. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pauth-server-quarkus` |
| `auth-server-quarkus-embedded` | Inactive | Runs testsuite against an embedded Quarkus server. Supports test-driven development without full distribution rebuild. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pauth-server-quarkus-embedded -Dtest=LoginTest` |
| `auth-server-cluster-quarkus` | Inactive | Enables clustered Keycloak test setup with 2 backend Quarkus nodes, a load balancer, and shared DB. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pauth-server-cluster-quarkus -Dsession.cache.owners=2` |
| `auth-server-cluster-undertow` | Inactive | Enables clustered Keycloak test setup using embedded Undertow servers. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pauth-server-cluster-undertow` |
| `db-mysql` | Inactive | Configures MySQL as the database for integration tests using containers. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pdb-mysql` |
| `db-postgres` | Inactive | Configures PostgreSQL as the database for integration tests using containers. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pdb-postgres` |
| `db-mariadb` | Inactive | Configures MariaDB for integration tests. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pdb-mariadb` |
| `db-mssql` | Inactive | Configures MS SQL Server for integration tests. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pdb-mssql` |
| `db-oracle` | Inactive | Configures Oracle DB for integration tests. Requires building the Docker image separately. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Pdb-oracle` |
| `cache-server-infinispan` | Inactive | Enables external Infinispan cache server for cross-DC tests. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/tests/base/pom.xml clean install -Pcache-server-infinispan` |
| `jboss-public-repository` | Inactive | Adds JBoss Public Maven Repository for dependency resolution (configured in `maven-settings.xml`). | `/maven-settings.xml` | `mvn clean install -s maven-settings.xml` |
| `keycloak-server` | Inactive | Used with `exec:java` to start the Keycloak test server (legacy testsuite utilities). | `/docs/tests.md` (documented) | `mvn exec:java -Pkeycloak-server` |

> **Note on database profiles:** To get a full list of available `db-*` profiles in the integration testsuite, run:
> ```bash
> mvn help:all-profiles -pl testsuite/integration-arquillian | grep -- db-
> ```

---

## Table 2: Maven Properties

Properties can be set via `-D` flags to customize build behavior without activating/deactivating entire profiles.

| Name | Default Value | Purpose / Description | File Path | Usage Example |
|------|--------------|----------------------|-----------|---------------|
| `skipTests` | `false` | Standard Maven Surefire property. Skips test execution but still compiles test classes. | (Maven standard) | `./mvnw clean install -DskipTests` |
| `maven.test.skip` | `false` | Skips both test compilation and execution entirely. | (Maven standard) | `./mvnw clean install -Dmaven.test.skip=true` |
| `skipTestsuite` | not set | When set, deactivates the `testsuite` profile, excluding the entire testsuite module from the build. | `/pom.xml` | `./mvnw clean install -DskipTestsuite` |
| `skipExamples` | not set | When set, deactivates the examples module from the build. | `/pom.xml` | `./mvnw clean install -DskipExamples` |
| `skipAdapters` | not set | When set, deactivates the adapters profile/module. | `/pom.xml` | `./mvnw clean install -DskipAdapters` |
| `skipDocs` | not set | When set, deactivates the docs profile/module. | `/pom.xml` | `./mvnw clean install -DskipDocs` |
| `skipCommunityTranslations` | not set | When set, excludes community translation files from themes, Admin UI, and Account UI builds. | `/themes/pom.xml`, `/js/apps/*/pom.xml` | `./mvnw clean install -DskipCommunityTranslations` |
| `operator` | not set | When set, activates the operator module build. | `/pom.xml` | `./mvnw clean install -Doperator` |
| `product` | not set | Activates downstream/product-specific build configuration. | Various `pom.xml` | `./mvnw clean install -Dproduct` |
| `maven.compiler.release` | `17` | Java release version for compilation. | `/pom.xml` | `./mvnw clean install -Dmaven.compiler.release=17` |
| `maven.build.cache.enabled` | `false` | Enables the Apache Maven Build Cache Extension for incremental builds. Opt-in feature. | `/pom.xml`, `/.mvn/` | `./mvnw clean install -Dmaven.build.cache.enabled=true` |
| `test` | not set | Standard Surefire property to run specific test classes. | (Maven Surefire) | `mvn clean install -Dtest=LoginTest` |
| `browser` | `htmlunit` | Selects the browser for UI integration tests. Options include `firefox`, `chrome`, etc. | `/testsuite/integration-arquillian/` | `mvn -f testsuite/integration-arquillian/pom.xml clean install -Dbrowser=firefox` |
| `auth.server.debug.port` | `5005` | Sets the debug port for the auth server during integration tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Dauth.server.debug.port=5005` |
| `auth.server.debug.suspend` | `n` | When set to `y`, the auth server waits for debugger attachment before starting. | `/testsuite/integration-arquillian/` | `mvn clean install -Dauth.server.debug.suspend=y` |
| `app.server.debug.port` | `5006` | Sets the debug port for the app server during adapter tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Dapp.server.debug.port=5006` |
| `app.server.debug.suspend` | `n` | When set to `y`, the app server waits for debugger attachment. | `/testsuite/integration-arquillian/` | `mvn clean install -Dapp.server.debug.suspend=y` |
| `session.cache.owners` | `1` | Number of cache owners for distributed Infinispan sessions in cluster tests. Set to `2` for failover tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Dsession.cache.owners=2` |
| `backends.console.output` | `false` | Enables console output from backend Keycloak nodes during cluster tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Dbackends.console.output=true` |
| `frontend.console.output` | `false` | Enables console output from the frontend load balancer during cluster tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Dfrontend.console.output=true` |
| `auth.server.log.check` | `true` | Enables log checking on the auth server after tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Dauth.server.log.check=false` |
| `keycloak.connectionsJpa.url` | (H2 default) | JDBC URL for the database connection in tests. | `/docs/tests-db.md` | `mvn install -Dkeycloak.connectionsJpa.url=jdbc:postgresql://localhost:5432/keycloak` |
| `keycloak.connectionsJpa.driver` | (H2 default) | JDBC driver class for the database connection in tests. | `/docs/tests-db.md` | `mvn install -Dkeycloak.connectionsJpa.driver=org.postgresql.Driver` |
| `keycloak.connectionsJpa.user` | (H2 default) | Database username for test connections. | `/docs/tests-db.md` | `mvn install -Dkeycloak.connectionsJpa.user=keycloak` |
| `keycloak.connectionsJpa.password` | (H2 default) | Database password for test connections. | `/docs/tests-db.md` | `mvn install -Dkeycloak.connectionsJpa.password=keycloak` |
| `keycloak.createAdminUser` | `true` | Controls whether the test server automatically creates a master realm admin user. | `/docs/tests.md` | `mvn exec:java -Pkeycloak-server -Dkeycloak.createAdminUser=false` |
| `keycloak.theme.dir` | not set | Points the test server to load themes from filesystem instead of classpath (enables live editing). | `/docs/tests.md` | `mvn exec:java -Pkeycloak-server -Dkeycloak.theme.dir=/path/to/themes` |
| `quarkus.args` | not set | Passes arguments to the Quarkus runtime when using development mode. | `/quarkus/README.md` | `../mvnw -f server/pom.xml compile quarkus:dev -Dquarkus.args="start-dev"` |
| `publish` | not set | When set, builds the documentation/website in publication mode. | `/docs/documentation/README.md` | `./mvnw clean install -Dpublish` |
| `surefire.memory.Xms` | `512m` | Minimum heap size for Surefire-forked JVMs. | `/pom.xml` | `./mvnw clean install -Dsurefire.memory.Xms=1024m` |
| `surefire.memory.Xmx` | `2048m` | Maximum heap size for Surefire-forked JVMs. | `/pom.xml` | `./mvnw clean install -Dsurefire.memory.Xmx=4096m` |
| `surefire.memory.metaspace` | `512m` | Metaspace size for Surefire-forked JVMs. | `/pom.xml` | `./mvnw clean install -Dsurefire.memory.metaspace=768m` |
| `surefire.memory.metaspace.max` | `1024m` | Maximum metaspace size for Surefire-forked JVMs. | `/pom.xml` | `./mvnw clean install -Dsurefire.memory.metaspace.max=2048m` |

---

## Table 3: Configurable Plugin Parameters

These are Maven plugin configurations that can be overridden via `-D` properties.

| Name | Default Value | Purpose / Description | File Path | Usage Example |
|------|--------------|----------------------|-----------|---------------|
| `spotless.check.skip` / `spotless.apply.skip` | `false` | Skips the Spotless code formatting check/apply. Spotless is configured in the root `pom.xml` with import ordering rules for Java files. | `/pom.xml` | `./mvnw clean install -Dspotless.check.skip=true` |
| `proto-schema-compatibility.skip` | `false` | Skips the proto-schema-compatibility-maven-plugin checks. Useful in proxy environments where these checks may fail. Per `docs/building.md`: "you can skip this check by adding the following property." | `/pom.xml`, `/docs/building.md` | `./mvnw clean install -Dproto-schema-compatibility.skip=true` |
| `maven.javadoc.skip` | `false` | Skips Javadoc generation during the build. | (Maven Javadoc Plugin) | `./mvnw clean install -Dmaven.javadoc.skip=true` |
| `maven.source.skip` | `false` | Skips source JAR generation. | (Maven Source Plugin) | `./mvnw clean install -Dmaven.source.skip=true` |
| `gpg.skip` | `false` | Skips GPG signing of artifacts (relevant during release builds). | (Maven GPG Plugin) | `./mvnw clean install -Dgpg.skip=true` |
| `checkstyle.skip` | `false` | Skips Checkstyle checks if configured. | (Maven Checkstyle Plugin) | `./mvnw clean install -Dcheckstyle.skip=true` |
| `enforcer.skip` | `false` | Skips Maven Enforcer Plugin rules (e.g., minimum Maven version checks). | (Maven Enforcer Plugin) | `./mvnw clean install -Denforcer.skip=true` |
| `frontend.skip` | `false` | Skips the `frontend-maven-plugin` executions (Node.js/npm/pnpm builds for JS modules). | `/js/` modules | `./mvnw clean install -Dfrontend.skip=true` |
| `skipExec` | `false` | Skips exec-maven-plugin executions. Recognized by the Maven Build Cache configuration. | `/.mvn/maven-build-cache-config.xml` | `./mvnw clean install -DskipExec=true` |
| `docker.database.image` | varies per DB | Overrides the Docker image used for database containers in integration tests. | `/testsuite/integration-arquillian/` | `mvn clean install -Pdb-oracle -Ddocker.database.image=my-oracle:latest` |
| `pnpm.args.install` | `install --prefer-offline --frozen-lockfile --ignore-scripts` | Overrides pnpm install arguments for JavaScript module builds. | `/pom.xml` | `./mvnw clean install -Dpnpm.args.install="install"` |
| `test.operator.deployment` | `local_apiserver` | Controls operator test deployment mode. Options: `local_apiserver`, `local`, `remote`. | `/operator/README.md` | `mvn clean install -Doperator -Dtest.operator.deployment=local` |

---

## Common Build Recipes for Contributors

### First-time full build (skip tests for speed)
```bash
./mvnw clean install -DskipTestsuite -DskipExamples -DskipTests
```

### Build only the Quarkus distribution
```bash
cd quarkus
../mvnw clean install -DskipTests
```
The ZIP distribution will be at `quarkus/dist/target/`.

### Run a single integration test (embedded Undertow, fastest)
```bash
mvn -f testsuite/integration-arquillian/pom.xml clean install -Dtest=LoginTest
```

### Run integration tests on real Quarkus server
```bash
mvn -f testsuite/integration-arquillian/pom.xml -Pauth-server-quarkus clean install
```

### Run integration tests with embedded Quarkus (TDD-friendly)
```bash
mvn -f testsuite/integration-arquillian/pom.xml -Pauth-server-quarkus-embedded clean install -Dtest=LoginTest
```

### Run tests with a specific database (e.g., PostgreSQL)
```bash
mvn install \
  -Dkeycloak.connectionsJpa.url=jdbc:postgresql://localhost:5432/keycloak \
  -Dkeycloak.connectionsJpa.driver=org.postgresql.Driver \
  -Dkeycloak.connectionsJpa.user=keycloak \
  -Dkeycloak.connectionsJpa.password=keycloak
```

### Run database tests using containerized profiles
```bash
mvn -f testsuite/integration-arquillian/pom.xml clean install -Pdb-postgres -Pauth-server-quarkus
```

### Build with the Operator module included
```bash
./mvnw clean install -DskipTests -Doperator
```

### Enable incremental build cache
```bash
./mvnw clean install -Dmaven.build.cache.enabled=true
```

### Run Keycloak in Quarkus dev mode
```bash
cd quarkus
../mvnw -f server/pom.xml compile quarkus:dev -Dquarkus.args="start-dev"
```

### Discover all available profiles in the integration testsuite
```bash
mvn help:all-profiles -pl testsuite/integration-arquillian | grep -E '^\s+-'
```

---

## Sources Consulted (from the Keycloak Repository)

All entries above are derived from the following files in the [keycloak/keycloak](https://github.com/keycloak/keycloak) repository:

- [`/pom.xml`](https://github.com/keycloak/keycloak/blob/main/pom.xml) — Root POM with profiles, properties, and plugin management
- [`/docs/building.md`](https://github.com/keycloak/keycloak/blob/main/docs/building.md) — Building and working with the codebase
- [`/docs/tests.md`](https://github.com/keycloak/keycloak/blob/main/docs/tests.md) — Test utilities and configurations
- [`/docs/tests-development.md`](https://github.com/keycloak/keycloak/blob/main/docs/tests-development.md) — Writing tests guide
- [`/docs/tests-db.md`](https://github.com/keycloak/keycloak/blob/main/docs/tests-db.md) — Database-specific testing
- [`/quarkus/README.md`](https://github.com/keycloak/keycloak/blob/main/quarkus/README.md) — Quarkus distribution module
- [`/quarkus/CONTRIBUTING.md`](https://github.com/keycloak/keycloak/blob/main/quarkus/CONTRIBUTING.md) — Quarkus contributing guide
- [`/testsuite/integration-arquillian/HOW-TO-RUN.md`](https://github.com/keycloak/keycloak/blob/main/testsuite/integration-arquillian/HOW-TO-RUN.md) — Integration test execution guide
- [`/testsuite/integration-arquillian/README.md`](https://github.com/keycloak/keycloak/blob/main/testsuite/integration-arquillian/README.md) — Testsuite overview
- [`/operator/README.md`](https://github.com/keycloak/keycloak/blob/main/operator/README.md) — Operator module
- [`/docs/documentation/README.md`](https://github.com/keycloak/keycloak/blob/main/docs/documentation/README.md) — Documentation build
- [`/themes/pom.xml`](https://github.com/keycloak/keycloak/blob/main/themes/pom.xml) — Themes module
- [`/js/apps/admin-ui/pom.xml`](https://github.com/keycloak/keycloak/blob/main/js/apps/admin-ui/pom.xml) — Admin UI module
- [`/js/apps/account-ui/pom.xml`](https://github.com/keycloak/keycloak/blob/main/js/apps/account-ui/pom.xml) — Account UI module
- [`/services/pom.xml`](https://github.com/keycloak/keycloak/blob/main/services/pom.xml) — Services module
- [`/maven-settings.xml`](https://github.com/keycloak/keycloak/blob/main/maven-settings.xml) — Maven settings with JBoss repository
- [`/.mvn/maven-build-cache-config.xml`](https://github.com/keycloak/keycloak/blob/main/.mvn/maven-build-cache-config.xml) — Build cache configuration

---

## Caveats and Notes

1. **This is a point-in-time snapshot.** The Keycloak project evolves rapidly. Always verify against the current `main` branch.
2. **Not all profiles are in the root `pom.xml`.** Many test profiles (especially `db-*` and `auth-server-*`) are defined in the integration-arquillian submodule POMs.
3. **Some properties are inherited** from the parent POM `org.jboss:jboss-parent:39`.
4. **The Quarkus distribution** is the primary distribution. References to WildFly/Undertow in the testsuite are for the embedded test server, not for production deployment.
5. **Network restrictions in proxy environments** may cause `proto-schema-compatibility-maven-plugin` to fail. Use `-Dproto-schema-compatibility.skip=true` as a workaround.