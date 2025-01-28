# Test Keycloak from source

## Background: Choosing the auth-server in use
For the arquillian-based test a keycloak server is started as part of the test. 

Maven-profiles with prefix 'auth-server' and name 'undertow' (default, so nothing selected), 'quarkus-server' and 'quarkus-server-embedded' select the keycloak server in use.
Switch away from default to 'quarkus-server' or 'quarkus-server-embedded' by selecting the maven-profile.
In IDE (IntelliJ) the plugin allows setting a profile for everything that is done inside the IDE.

Details on starting quarkus in this context is class 'AbstractQuarkusDeployableContainer'.

Hint: undertow will be removed.

## Regular task: Local: Run a single integration-test

### Commandline
```bash
./mvnw -f testsuite/integration-arquillian/pom.xml clean install -Dtest=LoginTest#loginSuccess
```

With auth-server profile: 
```bash
./mvnw -f testsuite/integration-arquillian/pom.xml clean install -Pauth-server-quarkus -Dtest=LoginTest#loginSuccess
```


### Run via IDE (IntelliJ)
Open class LoginTest.

Use IntelliJ to run e.g. 'loginSuccess' in LoginTest.

A proper build is required to have this working (do not use -DskipTestsuite when building)

## Regular task: Local: Debugging within testing

### Debug via IDE (IntelliJ)
Debugging is straightforward when using 'undertow' (default) or 'quarkus-server-embedded' profile.

Quarkus runs in a different JVM when using 'quarkus-server' profile.
To debug the code under test that is running inside a quarkus server started by the test-framework: use remote debugging.

System properties to add when starting a test:
```
-Dauth.server.debug=true
-Dauth.server.debug.port=5005
-Dauth.server.debug.suspend=y
```

### Command Line

Debugging a test, add
```bash
-Dmaven.surefire.debug=-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:8787
```
to commandline above.

Debugging code under test see properties above. 

## Regular task: Local: Coverage of a single test

### Run via IDE (IntelliJ)
Coverage from IDE is straightforward when using 'undertow' (default) or 'quarkus-server-embedded' profile.

Get coverage when running quarkus-server?

### Commandline
Any global coverage information available?

## Regular task: Local: Run a test with an external/already running keycloak server

Other kind of workflow ?