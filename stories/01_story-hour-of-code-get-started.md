# Story: Get ready to contribute code to Keycloak...
Keycloak code resides on GitHub.
Any contribution to anything inside https://github.com/keycloak require a pull request (pr). 
A pull request can be initiated/created through the browser/a web based ide or a local machine.

Keycloak repositories on GitHub contain a lot of [useful information regarding contributions](../howto-pointers.md).

This should probably not live long...

## Preparation

In general what is needed before contributing as necessary one time [preparation](../howto-00-prepare.md).

## Regular task: Local machine: preparation steps
1. Have an environment-variable CODE_HOME and KEYCLOAK_LOCAL_DIR in place
2. Have some "my-ssh-key-available" steps in place

```bash
export CODE_HOME=~/Local/repositories
export KEYCLOAK_LOCAL_DIR=keycloak_srose
export GITHUB_USER=srose
ssh-add ~/.ssh/id_rsa
```

## Local machine: Prepare local copy of keycloak using command-line
Alternatives:
- Use IDE (IntelliJ) GitHub-Integration 
- Use remote IDE
- ...

Notes: 
- It might be useful to have multiple local git repos for keycloak.
- Avoid commit/push something on main of your fork.

```bash
cd $CODE_HOME
```

Clone your fork...
```bash
git clone git@github.com:$GITHUB_USER/keycloak.git $KEYCLOAK_LOCAL_DIR
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
```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
```

Details for [keeping in sync](../howto-01-regular-sync.md)

## Regular task: Local: build on command-line
```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
```
Run a build command first, before opening an IDE is recommended.

Please also see some notes regarding [IntelliJ](../howto-02-ide-intellij.md)

All details on [building](../howto-02-build.md#command-line)

## Regular task: Local: open in IDE (IntelliJ)

Copy run-configurations from the [launch](../launch)-directory into current keycloak...

```bash
mkdir -p $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run
yes | cp -rf $CODE_HOME/keycloak_contributions/launch/* $CODE_HOME/$KEYCLOAK_LOCAL_DIR/.run/
```
```bash
cd $CODE_HOME/$KEYCLOAK_LOCAL_DIR
```

Run your IDE (IntelliJ) and open the current location or use the `Project from existing source` wizard.

Open the story-file we work with in **new IntelliJ-Window**.

Other IDE-build related info [here](../howto-02-build.md#ide-intellij)

## Regular task: Local: run keycloak on command-line

Different ways starting keycloak on a command-line in [here](../howto-03-run.md#command-line)

## Regular task: Local: run keycloak in IDE (IntelliJ)

### Run minimal

First run `keycloak-ju5-in-memory`...

Open http://0.0.0.0:8080/ in browser

Run a simple api call from [keycloak-api](../api/keycloak-discovery.http)

All [keycloak application parameters](https://www.keycloak.org/server/all-config) can be used in launch-config.

### Run with extensive Logging

Enable logging in the run-configuration to e.g. protocol

Stop previous launch

Run `keycloak-ju5-in-memory-logging`

Run a token call from [keycloak-api](../api/keycloak-client-credentials-grant.http)

### Debugging

Stop previous launch

**Debug** `keycloak-ju5-in-memory`

Set a breakpoint in e.g. TokenEndpoint.java

Run a token call from [keycloak-api](../api/keycloak-client-credentials-grant.http)

Step around after the breakpoint, e.g. add response-header in the instances window.

Other ways through remote debugging.

### Changed code (not only during debugging)

Change code: e.g. add response header.

Recompile the current File from Build-Menu.

Repeat token call from [keycloak-api](../api/keycloak-client-credentials-grant.http)

Without the debugger and for more complicated stuff, changes are either visible:
- immediately (IDE build)
- after restarting the launch-config
- a partial/complete rebuild through maven

### Persistence

By default, there is a [file based h2 database](https://www.keycloak.org/server/all-config#category-database) for persistence.

When using Keycloak from keycloak-junit5, everything regarding persistence resides in /target-folder to have it cleaned up via maven.

This can be changed via *kc.home.dir* setting to avoid being deleted via clean-goal of a maven run.

Another option of course would be one of the databases keycloak supports e.g. postgres, but requires a running postgres somewhere.

...

### Configuration

Through persistence, we would **not** lose our Clients, Users, Roles, Groups, etc.

Automation of this configuration could be an additional feature, but also reproduce our config explicitly.

...

### Run a specific version
For reproducing bugs or understanding behaviour in a previous release-version of keycloak.

[Sync tags](../howto-01-regular-sync.md#sync-tags-for-keycloak-releases) if they are missing.

```bash
git checkout <release-tag>  # e.g. 26.1.0, 26.4.7, etc.
```
Build and run as before...

### More on Running in the IDE(IntelliJ)
Not as a story documented [here](../howto-03-run.md#ide-intellij)
