# Story: Get ready to contribute code to Keycloak, 2026 edition

## Set up
Summary for the one time steps:
- Create your fork of keycloak in GitHub
- Set up your workspace to change code

All details on one time [preparation](../howto-00-prepare.md).

General set up
```bash
export CODE_HOME=~/repositories
cd $CODE_HOME
export GITHUB_USER=srose
ssh-add ~/.ssh/id_rsa
```

Start with a new clone?
```bash
export KEYCLOAK_LOCAL_DIR=keycloak_srose_devDay_26
```

Use an existing clone?
```bash
export KEYCLOAK_LOCAL_DIR=keycloak_srose_devDay_I
```

Initialize a new clone.
```bash
git clone git@github.com:$GITHUB_USER/keycloak.git $KEYCLOAK_LOCAL_DIR
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
git remote add upstream git@github.com:keycloak/keycloak
```

Details for [keeping an existing clone in sync](../howto-01-regular-sync.md)

## Building

Run a maven-build , before opening an IDE is recommended.

```bash
./mvnw clean install -DskipTests
```

TODO/Bug? Verify
We need to build TWO TIMES to have a working UI?

TODO/Bug? Error during build
```bash
[INFO] [io.quarkus.hibernate.orm.deployment.HibernateOrmProcessor] A legacy persistence.xml file is present in the classpath. This file will be used to configure JPA/Hibernate ORM persistence units, and any configuration of the Hibernate ORM extension will be ignored. To ignore persistence.xml files instead, set the configuration property 'quarkus.hibernate-orm.persistence-xml.ignore' to 'true'.
[ERROR] [io.smallrye.openapi.runtime.scanner.dataobject] SROAP31016: Unanticipated mismatch between type arguments and type variables declared on class
Class: io.smallrye.openapi.runtime.scanner.StreamStandin<E, S>
```

All details on [building](../howto-02-build.md#command-line)

Find [more profiles and flags](../howto-profiles-and-flags.md) assembled by some GenAI.

## Running Keycloak from the commandline

Run the local distribution? Not today.

I would like to use the Quarkus Development Experience.
Current issue: https://github.com/keycloak/keycloak/issues/46121

```
cd quarkus/server
../../mvnw compile quarkus:dev -Dkc.config.built=true -Dquarkus.args="start-dev"
```

## Switch to an IDE (today: IntelliJ)
The Keycloak project is correctly set up in the IDE after this step.

Please also see some notes regarding [IntelliJ](../howto-02-ide-intellij.md)

Some prepared launch-configurations for convenience:
```bash
mkdir -p $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run
yes | cp -rf $CODE_HOME/keycloak_contributions/launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```

Use the `New -> Project from existing source`-wizard.

Other IDE-build related info [here](../howto-02-build.md#ide-intellij)

## Running Keycloak from the IDE
See a properly styled page at http://localhost:8080 after this step.

Context for the new IDE terminal.
```bash
export KEYCLOAK_LOCAL_DIR=keycloak_srose_devDay_26
```

Open this guide in the new IDE window.

### JUnit5-Keycloak-Launcher
Run the config: `keycloak-ju5-in-memory`...

All [keycloak application parameters](https://www.keycloak.org/server/all-config) can be used in launch-config.

We need to build TWO TIMES to have a working UI? (TODO/Bug)

```bash
./mvnw clean install -DskipTests
```

### Quarkus App

Run the quarkus/server from within the IDE.
Current issue: https://github.com/keycloak/keycloak/issues/46121

Run `keycloak-quarkus-server-app`

## Debugging
Run keycloak / the underlying JVM in Debug Mode (Port 5005).

Run `debug-remote-5005` to connect the IDE.

Or just use the Debug Icon with the run configurations: eg. `keycloak-ju5-in-memory`

## Running integration tests
Examples to run integration tests.

Either the test starts Keycloak embedded or you need to prepare a running instance.

From the IDE e.g. for debugging just run the test via JUnit integration.

### Old Arquillian Testsuite

Skip today?

```bash
./mvnw -f testsuite/integration-arquillian/tests/base/pom.xml test -Dtest=LoginTest#loginSuccess
```

### Tests with the new Framework

Create an initial .env.test file to configure testing:
```bash
yes | cp -rf $CODE_HOME/keycloak_contributions/config/.env.test $CODE_HOME/$KEYCLOAK_LOCAL_DIR/
```

Run a single test from the command-line
```bash
./mvnw -f tests/base/pom.xml test -Dtest=WelcomePageTest#accessCreatedAdminAccount
```

To run the tests with an already running Keycloak adjust the setting in .env.test
```bash
KC_TEST_SERVER=remote
```

Credentials used by the tests:
```
org.keycloak.testframework.server.DistributionKeycloakServer#getServerInfo
```

### Integrate UI tooling

Reference the tools from the maven build
```bash
export PATH=$PATH:$CODE_HOME/$KEYCLOAK_LOCAL_DIR/js/node
```

### Account UI Tests
Via playframework UI

```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR/js/apps/account-ui
pnpm test -- --project=chromium --ui
```

or in the IDE run `ui-tests--account-ui`

### Admin UI Tests
Via playframework UI

```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR/js/apps/admin-ui
pnpm test:integration -- --project=chromium --ui
```

or in the IDE run `ui-tests--admin-ui`

## Start changing code?
You are set to start contributing code to Keycloak.

- [CONTRIBUTING.md in /](https://github.com/keycloak/keycloak/blob/main/CONTRIBUTING.md)

