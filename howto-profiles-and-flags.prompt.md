As a contributor I want to know what build-options I can use during the work on a contribution.

Search ONLY the keycloak git-repository for Maven build configurations in pom.xml files and provide them in a markdown table. Do not create a PR. This should include:
Consider the contents of all resources listed in the howto-pointers.md File from the keycloak_contributions repository. 
Do not consider the other documents content there, because it already contains assumptions. 
Everything in there must be challenged.
The keycloak git-repository is the only relevant source.

- Maven Profiles - Configurations activated with -P (e.g., mvn clean install -Pdev)
- Maven Properties - Properties that can be set with -D (e.g., mvn clean install -DskipTests=true)
- Configurable Plugin Parameters - Notable plugin configurations that accept property overrides

Create three markdown tables, one for each type: Profile/Property/Plugin Parameter.

The markdown tables should have the following columns:
- Name (e.g., profile ID, property name)
- Default-Value
- Purpose/Description (what it does)
- File Path (path to the pom.xml file)
- Usage Example (how to invoke it with mvn/mvnw)

Also search markdown (.md) and asciidoc (.adoc, *.asciidoc) files for mentions of:

Maven profiles being activated (e.g., -P flags)
Maven properties being set (e.g., -D flags)
Build commands using mvn or mvnw

Document any additional context found in the documentation about these build configurations.

Provide the result in a markdown file and add it to a branch in the git-repository.