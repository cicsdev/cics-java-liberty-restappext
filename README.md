cics-java-liberty-restappext
=============================

This repository provides sample materials for use with the IBM Redbooks video course
"[Extending a CICS Web application using JCICS](https://www.redbooks.ibm.com/redbooks.nsf/redbookabstracts/crse0302.html?Open)". The
application provided is a simple, RESTful web application providing several code examples for accessing CICS resources from Java using
the JCICS API.

This repository builds on the application constructed in the IBM Redbooks video course
"[Developing a RESTful Web application for Liberty in CICS](https://www.redbooks.ibm.com/redbooks.nsf/redbookabstracts/crse0300.html?Open)",
which uses the [cics-java-liberty-restapp](https://github.com/cicsdev/cics-java-liberty-restapp) repository.

For further examples, see the [cics-java-jcics-samples](https://github.com/cicsdev/cics-java-jcics-samples) repository.

## Repository contents

Full details on the contents of this repository can be found on the [Source code](Source.md) page.

## Pre-reqs

* CICS TS V5.4 or later
* Java SE 1.8 or later on the workstation
* Eclipse with the IBM CICS SDK for Java EE, Jakarta EE and Liberty, or any IDE that supports usage of the Maven Central artifact [com.ibm.cics:com.ibm.cics.server.](https://search.maven.org/artifact/com.ibm.cics/com.ibm.cics.server)
* Maven or Gradle build tools (optional)


## Configuration

The sample Java classes are designed to be added to a dynamic web project and deployed into a Liberty JVM server as a WAR,
either using the dropins directory or using a CICS bundle project. 

The VSAM examples use the sample file `SMPLXMPL`. For a sample CICS FILE definition, see the file [`etc/DFHCSD.txt`](etc/DFHCSD.txt).

### To add the resources to Eclipse:
1. Using an Eclipse development environment create a dynamic web project called `com.ibm.cicsdev.restappext` and add the Java samples to the `src` folder.
1. Copy the `com.ibm.cicsdev.restappext.generated.jar` file to the folder `/WebContent/WEB-INF/lib` relative to the root of your WAR project.
1. Add the CICS Liberty JVM server libraries to the build path of your project. 
1. Add the `com.ibm.cicsdev.restappext.generated.jar` file to the project build path.
1. Ensure the web project is targeted to compile at a level that is compatible with the Java level being used on CICS. This can be achieved by editing the Java Project Facet in the project properties.
1. [Optional] Create a CICS bundle project called `com.ibm.cicsdev.restappext.cicsbundle` and add a dynamic web project include for the project created in step 1.



## Building the Example

The sample can be built using the supplied Gradle or Maven build files to produce a WAR file and optionally a CICS Bundle archive.

### Building with Gradle

A WAR file is created inside the `build/libs` directory and a CICS bundle ZIP file inside the `build/distributions` directory.

If using the CICS bundle ZIP, the default CICS JVM server name `DFHWLP` should be modified using the `cics.jvmserver` property in the gradle build [file](build.gradle) to match the required CICS JVMSERVER resource name, or alternatively can be set on the command line.

| Tool | Command |
| ----------- | ----------- |
| Gradle Wrapper (Linux/Mac) | ```./gradlew clean build``` |
| Gradle Wrapper (Windows) | ```gradle.bat clean build``` |
| Gradle (command-line) | ```gradle clean build``` |
| Gradle (command-line & setting jvmserver) | ```gradle clean build -Pcics.jvmserver=MYJVM``` |

### Building with Maven

First install the generated JAR file into a local Maven repository by running the following Maven command in a local command prompt

`mvn org.apache.maven.plugins:maven-install-plugin:3.1.3:install-file -Dfile=lib/cics-java-liberty-restappext-generated.jar -DgroupId=com.ibm.cicsdev -DartifactId=cics-java-liberty-restappext-generated -Dversion=1.0 -Dpackaging=jar -DlocalRepositoryPath=local-repo`

Next, run the appropriate Maven build command. This creates a WAR file in the `target` directory. 

If building a CICS bundle ZIP the CICS bundle plugin bundle-war goal is driven using the maven verify phase. The CICS JVM server name should be modified in the `<cics.jvmserver>` property in the [`pom.xml`](pom.xml) to match the required CICS JVMSERVER resource name, or alternatively can be set on the command line. 

| Tool | Command |
| ----------- | ----------- |
| Maven Wrapper (Linux/Mac) | ```./mvnw clean verify``` |
| Maven Wrapper (Windows) | ```mvnw.cmd clean verify``` |
| Maven (command-line) | ```mvn clean verify``` |
| Maven (command-line & setting jvmserver) | ```mvn clean verify -Dcics.jvmserver=MYJVM``` |

## Deployment 
### To start a JVM server in CICS:

1. Define a Liberty JVM server called `DFHWLP` using the supplied sample definition `DFHWLP` in the CSD group `DFH$WLP`.
1. Copy the CICS sample `DFHWLP.jvmprofile` zFS file to the `JVMPROFILEDIR` directory specified above and ensure the `JAVA_HOME` variable is set correctly.
1. Add the `jaxrs-1.1` Liberty feature to `server.xml`.
1. Install the `DFHWLP` JVMSERVER resource defined in step 1 and ensure it becomes enabled.
1. Add the `cicsts:link-1.0` feature to `server.xml` to enable Link to Liberty.

### To add sample resources to CICS:
1. Compile the supplied sample COBOL programs into a load library included in the CICS DFHRPL concatenation.
1. Run a DFHCSDUP job using the definitions for the [sample resources](etc/DFHCSD.txt).


## Running the code samples

See the dedicated pages for executing the [JAX-RS](RunningJAXRS.md) and [Link to Liberty](RunningLinkToLiberty.md) sample applications inside CICS.

## License
This project is licensed under [Apache License Version 2.0](LICENSE).

