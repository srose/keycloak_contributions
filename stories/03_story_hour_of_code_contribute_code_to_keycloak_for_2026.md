# Story: Get ready to contribute code to Keycloak, 2026 edition
- What makes Keycloak Contributions hard? challenging?
  - its a big project with a lot of technology in use?
  - wildfly/quarkus, so it did not start with quarkus? 
  - Tons of Features, Toggels, etc.

- Not contributed yet: know how to start...
  - Initial Setup
- What has changed from a contributors' perspective since last year?


- Everything we already had, but in different words.
- Run Keycloak and interact with it
- Running all kind of tests locally (and debug)

## Set up
Summary for the one time steps:
- Create your fork of keycloak in GitHub
- Set up your workspace to change code

All details on one time [preparation](../howto-00-prepare.md).

General set up
```bash
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk
export CODE_HOME=~/repositories
export GITHUB_USER=srose
ssh-add ~/.ssh/id_rsa
cd $CODE_HOME
```

Start with a new clone?
```bash
export KEYCLOAK_LOCAL_DIR=keycloak_srose_devDay26
```

Use an existing clone?
```bash
export KEYCLOAK_LOCAL_DIR=keycloak_srose
```

Initialize a new clone.
```bash
git clone git@github.com:$GITHUB_USER/keycloak.git $KEYCLOAK_LOCAL_DIR
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
git remote add upstream git@github.com:keycloak/keycloak
```

On a regular basis you need to sync your fork.

```bash
cd CODE_HOME/$KEYCLOAK_LOCAL_DIR
CURRENT_BRANCH=`git rev-parse --abbrev-ref HEAD`
git stash
git checkout main
git fetch upstream
git rebase upstream/main
git push origin main
git checkout $CURRENT_BRANCH
git stash pop
```

Issues with node_modules?
```bash
rm -rf js/node_modules
```

Details for [keeping in sync](../howto-01-regular-sync.md)

## Building

Run a build command first, before opening an IDE is recommended.

```bash
./mvnw clean install -DskipTests -DskipTestsuite -DskipExamples -DskipDocs
```

Error/Bug?
We need to build two times to have reproducible behavior ?

Error/Bug?
```bash
[INFO] [io.quarkus.hibernate.orm.deployment.HibernateOrmProcessor] A legacy persistence.xml file is present in the classpath. This file will be used to configure JPA/Hibernate ORM persistence units, and any configuration of the Hibernate ORM extension will be ignored. To ignore persistence.xml files instead, set the configuration property 'quarkus.hibernate-orm.persistence-xml.ignore' to 'true'.
[ERROR] [io.smallrye.openapi.runtime.scanner.dataobject] SROAP31016: Unanticipated mismatch between type arguments and type variables declared on class
Class: io.smallrye.openapi.runtime.scanner.StreamStandin<E, S>
```

All details on [building](../howto-02-build.md#command-line)

Find [more profiles and flags](../howto-profiles-and-flags.md) assembled by some GenAI.

## Running Keycloak from the commandline

Run the local distribution? Not today.

Error/Bug?
Would love to use the Quarkus Development Experience, but does not work at the moment?
```
cd quarkus/server
../../mvnw compile quarkus:dev -Dkc.config.built=true -Dquarkus.args="start-dev"
```

For today: just use the IDE?

## Switch to an IDE

Please also see some notes regarding [IntelliJ](../howto-02-ide-intellij.md)

For convenience:
```bash
mkdir -p $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run
yes | cp -rf $CODE_HOME/keycloak_contributions/launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```

Run your IDE (IntelliJ) and open the current location or use the `Project from existing source` wizard.

Other IDE-build related info [here](../howto-02-build.md#ide-intellij)

## Running Keycloak from the IDE

### JUnit5-Keycloak-Launcher
Run the config: `keycloak-ju5-in-memory`...

or

run `keycloak-ju5-in-memory-logging`'

### Quarkus App?



## Debugging (makes no sense without IDE?)
Run keycloak in Debug Mode just from the distribution

Or from the previous section, from our build.

At the end, the JVM process is open for the debugger to connect.

Or just use the Debug Icon with the run configurations: eg. `keycloak-ju5-in-memory`

## Running integration tests
Examples to run tests against the local Keycloak instance, that might run in debug mode.

### Tests with the new Framework

Create an initial .env.test file
```bash
yes | cp -rf $CODE_HOME/keycloak_contributions/config/.env.test $CODE_HOME/$KEYCLOAK_LOCAL_DIR/
```

Run a single test
```bash
./mvnw -f tests/base/pom.xml test -Dtest=RealmRemoveTest
```

### Old Arquillian Testsuite

Skip today?

```bash
./mvnw -f testsuite/integration-arquillian/tests/base/pom.xml test -Dtest=LoginTest
```

### Account UI Tests
```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR/js/apps/account-ui
pnpm test -- --project=chromium --ui
```

### Admin UI Tests
```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR/js/apps/admin-ui
pnpm test:integration -- --project=chromium --ui
```

## Start changing code?

## Outlook
You are set to start contributing code to Keycloak.
