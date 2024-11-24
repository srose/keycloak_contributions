# Contributions 
Keycloak code resides on GitHub.
Any contribution to anything inside https://github.com/keycloak require a pull request (pr). 
A pull request can be initiated/created through the browser/a web based ide or a local machine.

## Local machine: preparation steps
1. Install & configure git 
2. Install Java 21
5. Optional: Install Maven
   - Maven wrapper in keycloak root, but useful for subproject runs
6. Generate ssh-key into ~/.ssh/
   - Alternative: just use https (in preparation)

## Regular task: Local machine: preparation steps
1. Have an environment-variable CODE_HOME and KEYCLOAK_LOCAL_DIR in place
2. Have some "my-ssh-key-available" steps in place

```bash
export CODE_HOME=~/Local/repositories
export KEYCLOAK_LOCAL_DIR=keycloak_my
ssh-add ~/.ssh/id_rsa
```

## Github: initialization steps
1. Create a github account: https://github.com/signup
2. Setup your github account with an ssh-key: https://github.com/settings/keys

## Github: prepare git repo to contribute to
1. Create a fork of the keycloak-repository
   - https://github.com/keycloak/keycloak/fork
2. See that fork
   - https://github.com/<github-handle>/keycloak
   - https://github.com/srose/keycloak

## Local: prepare git repo
Notes: 
- It might be useful to have multiple local git repos for keycloak.
- Avoid commit/push something on main of your fork.

```bash
cd $CODE_HOME
```

Clone your fork...
```bash
git clone git@github.com:srose/keycloak.git $KEYCLOAK_LOCAL_DIR
cd $KEYCLOAK_LOCAL_DIR
```

Connect upstream to keycloak repo the fork came from...
```bash
git remote add upstream git@github.com:keycloak/keycloak
```

Optional: Connect other upstreams to other forks
```bash
git remote add upstream_ahus1 git@github.com:ahus1/keycloak
```

Check all remotes:
```bash
git remote -v
```

## Regular task: Local: keep in sync with central keycloak
Skip today

```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
```

Details for [keeping in sync](../howto-00-regular-sync.md)

## Regular task: Local: start a new PR for an issue
Skip today

## Regular task: Local: keep in sync with local work or even a PR in progress
Skip today

## Regular task: Local: finish a PR for an issue
Skip today

## Regular task: Local: build on commandline
```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
```

Favorit build command, ...
```bash
./mvnw clean install -DskipTests
```

... but there is a lot more on [building](../howto-01-build.md#commandline) on commandline

## Regular task: Local: open in IDE (IntelliJ)

Copy run-configurations from the [launch](./launch/)-directory into current keycloak...

```bash
mkdir -p $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run
yes | cp -rf $CODE_HOME/keycloak_contributions/launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```

Current story-file to clipboard.

```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
/opt/idea/bin/idea.sh .
```

Start a new terminal.

```bash
ssh-add ~/.ssh/id_rsa
```

Open the current story-file.

Other IDE-build related info [here](../howto-01-build.md#ide-intellij)

## Regular task: Local: run keycloak on commandline
Skip today

## Regular task: Local: run keycloak in IDE (IntelliJ)

### Run

First run `launcher-in-memory`...

Open http://localhost:8080 in browser

Run a simple api call from [keycloak-api](../api/keycloak-discovery.http)

### Run with extensive Logging

Enable logging in the run-configuration to e.g. protocol

Stop previous launch

Run `launcher-in-memory-logging`

Run a token call from [keycloak-api](../api/keycloak-client-credentials-grant.http)

### Debug

Stop previous launch

Run `mvn-quarkus-dev-debug`

Debug `debug-remote-5005`

Set a breakpoint in e.g. TokenEndpoint.java

Run a token call from [keycloak-api](../api/keycloak-client-credentials-grant.http)

Step around in the breakpoint, set e.g. add response-header.

### Reload changed code?

Change code: e.g. add response header.

Recompile from Build-Menu: (Ctrl+Shift+F9|Сmd+Shift+F9)

Repeat token call from [keycloak-api](../api/keycloak-client-credentials-grant.http)

### Run a specific version
For reproducing bugs or understanding behaviour in a previous version.

[Sync tags](../howto-00-regular-sync.md#sync-tags-for-keycloak-releases) if they are missing.

```bash
git checkout 26.0.1
```

Build and run as before...

### Continue
See more examples [here](../howto-02-run.md#ide-intellij)
