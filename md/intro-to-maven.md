#Intro to Maven
-
-
#Maven Objectives
  <p class="fragment fade-up">- Making the build process easy</p>
  <p class="fragment fade-up">- Providing a uniform build system</p>
  <p class="fragment fade-up">- Providing quality project information</p>
  <p class="fragment fade-up">- Providing guidelines for best practices development</p>
  <p class="fragment fade-up">- Allowing transparent migration to new features</p>
-
# What Does Maven Do?
  <p class="fragment fade-up">- Handles multiple jars</p>
  <p class="fragment fade-up">- Handles dependencies and versions</p>
  <p class="fragment fade-up">- Project Structure</p>
  <p class="fragment fade-up">- Build, Publish, Deploy, Report, Documentation</p>

-
-
# Archetpyes
  An archetype is defined as an original pattern or model from which all other things of the same kind are made.
  <a href="https://maven.apache.org/guides/introduction/introduction-to-archetypes.html">maven achetypes</a>
-
#archetype:generate
  <ul>
    <li>Archetype</li>
    <li>Group ID</li>
    <li>Artifact ID</li>
    <li>Version</li>
    <li>Package</li>
  </ul>
  <aside class="notes">
    archetype - info to maven about type of archetype needed(project structure)
    groupId - similar to package name(unique identifier) usually a reserved domain
    artifactId - name of output artifact(jar, war, ear)
    package - name of package where artifact needs to assigned
  </aside>
-
-
# The pox.xml file
  <p class="fragment fade-up">(Project Object Model)</p>
  <p class="fragment fade-up">- Project details</p>
  <p class="fragment fade-up">- Project properties</p>
  <p class="fragment fade-up">- Project dependencies</p>
  <p class="fragment fade-up">- Project build profiles</p>
  <aside class="notes">
    It is an XML file that contains information about the project and configuration details used by Maven to build the project.
  </aside>
-
#pox.xml
  ![](img/intro-to-maven/pomFile.png)
-
-
#Maven Build Process
  <ul align="left">
    <li>validate</li>
    <li>compile</li>
    <li>test</li>
    <li>package</li>
  </ul>
  <ul align="right">
    <li>integration-test</li>
    <li>verify</li>
    <li>install</li>
    <li>deploy</li>
  </ul>
  <aside class="notes">
    validate - Validates whether project is correct and all necessary information is available to complete the build process.

    compile - Compile the source code of the project.

    test - Run tests using a suitable unit testing framework(Junit is one).

    package - Take the compiled code and package it in its distributable format, such as a JAR, WAR, or EAR file.

    integration-test - Process and deploy the package if necessary into an environment where integration tests can be run.

    verify - Run any check-ups to verify the package is valid and meets quality criterias.

    install - 	Install the package into the local repository, which can be used as a dependency in other projects locally.

    deploy - Copies the final package to the remote repository for sharing with other developers and projects.
  </aside>
-
-
#Adding a Dependency
[Adding a dependency through IntelliJ IDEA](https://www.jetbrains.com/help/idea/2017.1/downloading-libraries-from-maven-repositories.html)

Adding a dependency through [mvnrepository.com](https://mvnrepository.com/artifact/org.slf4j/slf4j-api/1.7.22)
