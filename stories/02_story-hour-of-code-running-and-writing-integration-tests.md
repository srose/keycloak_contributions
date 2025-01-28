# Story: Running and writing integration tests
When contributing a fix to the Keycloak Open Source code base in a pull request, you'll see a lot of tests that are executed using GitHub actions. 
Hopefully all of them still pass after your changes, and you'll soon be asked to contribute a test that shows your fix works as expected.

This talk provides you an overview on which kind of tests exists, how to run them locally and how to contribute to them. 
This will get you one step closer to contribute to Keycloak.

## Prerequisite

Have your local repository ready as prepared in the [getting started](./01_story-hour-of-code-get-started.md).

Note: IDELauncher is not working any longer ~ 14.12.24.

## Overview
Starting with general purpose testing for quality assurance for a code change to keycloak.
In contrast to even more very interesting topics for special purpose testing.
These are e.g. performance tests, standard-conformance-tests, etc.
Each of them are a topic on their own.

We stick to the general purpose testing for quality assurance of a code change to keycloak.
Depending on the introduced changes different tests might be affected.
Place to find theses: look into the pipeline of a pull-request to see what tests are executed there.

Once we cover UI development we'll learn on UI-e2e-tests.
This is also a topic on its own.
Focus for today is on the Java-based parts.
In the Java based parts there are a few unit tests and a lot of integrations tests.
A statement on [test-developement](https://github.com/keycloak/keycloak/blob/main/docs/tests-development.md) explains the way keycloak-developers agreed on the topic.
The test-development doc also adds details where to find the integration tests and details on their workings.

The integration test framework in use is called arquillian.
Everything keycloak can do, covered through the tests - awesome!
Hm, [arquillian](https://arquillian.org/) looks somehow discontinued? [Github-repos](https://github.com/arquillian) at least has changes.
A [new test-framework](https://www.keycloak.org/2024/11/preview-keycloak-test-framework.html) was introduced, but that is a topic on its own too.
Same with migrating from old to new way of testing.

Bigger amounts of tests take a long time and there might be 'local' issues when running from IDE or command-line, so it is fine to run them in Github.

## Get into our first test
The documentation recommends to start here: 'LoginTest.java'

LoginTest is an integration test around the Login-Form using several pages.
Each form is wrapped as a page-object. 
[Arquillian](https://arquillian.org/) fosters executing the code as is, so a [keycloak service](../howto-04-test.md#choosing-the-auth-server-in-use) will start as part of the test.

Details on [running a test](../howto-04-test.md#regular-task-local-run-a-single-integration-test)

## Debugging during testing

Debugging a test is straight forward from the IDE (IntelliJ) running on 'undertow' or 'quarkus-embedded'.

Details on [debugging during a test](../howto-04-test.md#regular-task-local-debugging-within-testing)

## Good examples

Some examples from [Git-History](https://github.com/keycloak/keycloak/pulls?q=is%3Apr+is%3Aclosed) to learn from others
1. [Fix access token issue OID4VC](https://github.com/keycloak/keycloak/pull/31763/files)
2. [Token exchange - added experimental token exchange V2 divided into multiple features](https://github.com/keycloak/keycloak/pull/36407/files)
3. ...

## Contribute test/parts of tests only 

Some ideas...
1. Find something not yet covered as part of an existing test via [coverage](../howto-04-test.md#regular-task-local-coverage-of-a-single-test).
2. See if the structure of test-code can be improved.
3. Parts of tests that are not necessary because things are already tested somewhere else.
4. ...

## Contribute a feature
My example change: how to choose the right test?

## Awesome util-collection
Starting in [tests.md in /docs](https://github.com/keycloak/keycloak/blob/main/docs/tests.md#totp-codes) 

External dependencies like Mail, LDAP, ...

## Summary
Contributed changes must be covered by tests?
From the [official contribution-guide in keylcoak](https://github.com/keycloak/keycloak/blob/main/CONTRIBUTING.md): "Includes functional/integration test"

Do your best to cover proposed changes, as we all like "surprise free" releases.




