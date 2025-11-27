+++
title = "Apache Maven in Depth"
date = ""
description = ""
+++


## What is Maven?

Maven...

### Maven is a build tool

    ✅ it creates deployable artifacts of source code 


    ✅ it facilitates automated/repeatable builds


    ✅ it facilitates deploying artifacts on servers

### Maven is a dependency management tool

    ✅ it can download project dependencies from centralized repositories ( e.g. [mvnrepository](https://www.mvnrepository.com))


    ✅ it can automatically resolve the libraries required by project dependencies


    ✅ it can do "Dependency Scoping" (e.g. it ensures that a dependency is only used during integration tests and not in production).


### Maven is a project management tool

    ✅ Artifact Versioning

    ✅ Change Logs

    ✅ Documentation

    ✅ JavaDocs

    ✅ Reports

### Maven is a standardized approach to building software

    ✅ Uniformity across projects through patterns (e.g. standard directory structure: it dictates the structure of your project - which is nice)


    ✅ Maven ist „Opinionated“ (= Maven has strong opinions about how projects should be structured and built).


    ✅ Convention over Configuration


    ✅ Consistent Path for all projects


    ✅ [philosophy-of-maven](https://maven.apache.org/background/philosophy-of-maven.html)

### Maven is a Command line tool

    ✅ you can communicate with maven via the command line (e.g. `mvn install`)








## Project Object Model (POM)

Pasted Graphic.png





*Repositories:*

= Collection of artifacts or dependencys that can be local or remote.

Local repositories: hat man lokal am laptop (kann man sich wie einen lokalen Cache vorstellen - The default location of the local repository is the . m2/repository/ directory under the user's home directory)
Remote repositories: z.b. mvnrepository.com
Wenn man eine remote repository runterlädt, wird sie im lokalen repository abgelegt.
Das lokale Repository hat bei der Auflösung von Abhängigkeiten Vorrang -> es wird also immer zuerst das lokal gespeicherte hergenommen.


*Plugins and Goals:*

Plugins kann man von remote repositories runterladen. Ein Plugin ist eine Collection of Goals. 

Goals perform the actions in maven builds -> all work is done via plugins and goals. they are called independently or as part of a lifecycle phase.



Kann man sich besser so vorstellen:

Eine Analogie zu Java-Klassen wäre, dass ein Plugin  eine Klasse ist und die dazugehörigen goals sind Methoden in dieser Klasse.

Beispiel:



Das Maven Compiler Plugin hat zwei Goals:

compile source
compile test


goals are operations that we are performing against our codebase to  build a product. All the work that we do with maven is done through plugins and goals.



Each plugin and its goals can bind to a particular phase in a lifecycle. 



*Lifecycle and Phases*:

Maven has 3 Lifecycles: Clean, default and site

Phases are executed sequentially and executing a phase executes all previous phases. Default is by far the largest lifecycle (it has about 23 phases in it).

z.b. as we execute the default lifecycle and we enter into the compile phase we are going to kick of the compile sources goal on our compiler plugin.



*Maven Technical Overview*:



executing the install phase on maven:

„mvn install“



dann passiert folgendes:



Pasted Graphic 1.png

maven examines pom.xml file (it looks for project information, dependencies and plugins)
in order to obtain those dependencies/plugins, maven uses its dependency manager. The dependency manager looks at the dependencies and plugins listed in the pom and then pulls (downloads) it from the different repositories (maven central, nexus, etc…)
Now we have the dependencies/plugins available locally. Once they are available, the lifecycle will begin.
Weil wir die install-Phase aufgerufen haben, beginnen wir mit der Ausführung der compile-Phase. Dafür müssen wir alle Phasen ausführen, die der install-Phase vorausgehen. 






POM.xml structure:

<project> —> root tag of all pom.xml files



<modelVersion>  —> indicate what version of the object model this pom is using. For Maven 2 and Maven 3 this model version is always „4.0.0“ (this does not change very often).



<artifactId> —> this is the name of the artifact we are going to generate with our project



<groupId>  —> indicates the unique identifier of the organization or the group that is creating the project.



artifactId and groupId are used to pull down plugins and dependencies. This is the adress or coordinate system of maven.



<version> —> version number of the project. When we work on a project that is under development, this is usually suffixed with „SNAPSHOT“. Sometimes RELEASE is used as suffix when a release is put out for general availability. This version almost serves as a timestamp of the application.



<packaging> —> indicates the type of packaging that we will use for the type of artifact we will produce. z.B. jar, war, etc. the default of this element is „jar“. So if you do not specify this packaging tag, it is automatically set to jar. you can leave it out.



More tags for additional information:

<name>

<description>

<url>

<license>

<organization>

<developers>



——————————



Beispiel:



mvn package. —> execute the package phase within the default lifecycle. This phase will create a jar file which contains the contents of our project. Jede Phase, die der package-Phase vorausgeht – wie zum Beispiel compile und test –, wird ausgeführt, wenn wir die package-Phase ausführen.



—————————————— 



*Standard directory structure*



Wenn ich mvn compile ausführe, dann kompiliert maven das File Application1.java zu Application1.class:

Pasted Graphic 2.png

 Aber woher weiß maven, wo Application1.java liegt? Es wurde ja nirgends im Pom-file angegeben. (think)



Answer:

Maven relies on Convention over Configuration!

Instead of specifying the location of our source files, we simply place our java source files within a pre defined location that maven uses as a standard. This is known as the maven standard directory layout. There are several other directorys specified by maven: https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html  



Theoretisch könnte man diese struktur überschreiben, indem man folgendes ins pom.xml schreibt:

Pasted Graphic 3.png

aber das sollte man niemals machen!!! Es ist nicht recommended!!



*Inheritance*

In java können klassen voneinander erben. Dasselbe Konzept gibt es auch in Maven. In Maven können POM.xml files voneinander erben. The configuration of the parent pom is then carried over to the child pom. In the child pom, we can then override some of the configuration specified in the parent pom. Similarly to java inheritance where every class inherits from the Object class —> Every pom file inherits from a super pom file.

If you wanna see the entire pom (the child pom + inherited super pom), you have to execute the command „mvn help:effective-pom“.



So lässt man zwei pom.xml files voneinander erben:



man erstellt zwei projekte mit jeweils eigenen pom.xml files:
Pasted Graphic 5.png



im child-pom pezifiziert man dann einen  <parent> tag in dem die coordinates von jenem pom file sind, von dem man erben möchte. z.B.:





Pasted Graphic 6.png



dann kann man im parent projekt „mvn install“ ausführen, damit das maven parent pom in das lokale repository gelegt wird.



wenn man jetzt im child projekt „mvn help:effective-pom“ ausführt, dann sieht man, wie die Vererbung funktioniert hat.





*Profiles*:
Maven Profiles allow us to build our projects for different environments. It gives us a mechanism to customize our builds depending upon the environment we are targeting. So within our pom file, we are able to specify a profile which will define an alternative set of configuration for any particular environment.



Beispiel: Wir haben ein pom file, in dem definiert wird, dass das jar file nach dem builden der Applikation in einem speziellen Ordner abgespeichert wird. Nun kann man mit profiles definieren, dass in den unterschiedlichen environments das jar file in unterschiedlichen ordnern abgelegt werden:



Pasted Graphic 7.png



(Man kann aber natürlich nicht nur den build tag wie in dem beispiel in profiles definieren sondern alle anderen Sachen auch.)



Beim ausführen der commands kann man dann das profil mit -P „mitgeben“: „mvn -Pproduction package“. Wenn man das so ausführt, dann wird das file in den production ordner gelegt und nicht in den development ordner.





Activator:

We dont always want to define the profile we’d like to apply by using this command line flag when accessing a goal. Sometimes it is not possible to hand over the flag. For this case, activators come into play. An activator is a mechanism to look at something else than this flag. For instance an environment variable. So in the following example, the profile is set based on an environment variable:



Pasted Graphic 8.png



you also have to specify the path variable name (<name>) and the path variable value (<value>). Also hier wird entschieden welches profil hergenommen wird basierend darauf, was in der environment variable steht.



dann muss man nur mehr die environment variable am computer anlegen und „mvn package“ ausführen. 



ℹ️ there are 5 different activators we can apply:

activeByDefault —-> set a profile as active by default

file. —-> looks if a specific file is available or not

jdk —> considers what version of java you are using to build with 

os —> decides based on the operating system you are working on

property —-> decides based on environment variables



———————————————————————

Maven Archetype Plugin

mvn archetype:generate

Archetype = pre defined projects (templates that you can use to build your own project)

is used to create new maven projects very fast.

-----------------------------------------------

## maven's dependency management

dependency management is pulling in the different libraries that you need for your applications from a repository.

1. pom file specify coordinates of a dependency
2. maven looks into the local repository and sees if it exists
3. if not it pulls down the dependency from a remote repository and caches it in the local repository

im pom file gehören die dependencys in den dependencies tag. z.B.:

<dependencies>
<dependency>
<groupId>org.apache.commons</groupId>
<artifactId>commons-lang3</artifactId>
<version>3.3.2</version>
</dependency>
</dependencies>

execute the copy-dependencies goal on the dependency plugin:

mvn dependency:copy-dependencies

this will tell maven to pull down the dependencies for our project and place them within our local repository.


## versionsnummern in propertys auslagern

man kann versionsnummern auch in propertys auslagern. z.b. so:

<properties>
  <slf4j.version>2.0.13</slf4j.version>
</properties>

<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>${slf4j.version}</version>
</dependency>



## keine version angeben

Die 3 möglichen Fälle:

1️⃣ Version kommt aus dem <dependencyManagement> eines Parent-POMs

Das ist der Standard- und empfohlene Fall:

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>2.0.13</version>
        </dependency>
    </dependencies>
</dependencyManagement>

Dann kannst du in einem Kind-POM schreiben:

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <!-- keine Version -->
</dependency>

✅ Maven weiß, dass die Version 2.0.13 aus dem dependencyManagement kommt.
Das ist der einzige saubere Weg, um eine Dependency ohne Version zu deklarieren.


2️⃣ Version kommt aus einem „BOM“ (Bill of Materials)

Das ist eine moderne Variante (z. B. bei Spring Boot oder Micronaut):


<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.micronaut</groupId>
            <artifactId>micronaut-bom</artifactId>
            <version>4.4.2</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

Dann kannst du alle Micronaut-Dependencies ohne Version definieren:

<dependency>
    <groupId>io.micronaut</groupId>
    <artifactId>micronaut-http-client</artifactId>
</dependency>

✅ Maven holt sich die Version aus der BOM, die über <dependencyManagement> importiert wurde.

3️⃣ Es gibt keine Quelle für die Version

Wenn du das machst:

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
</dependency>

...und nirgendwo (kein Parent, kein BOM, kein <dependencyManagement>) eine Version definiert ist, dann:

🚫 Baut Maven nicht.

Du bekommst einen Fehler wie:

[ERROR] 'dependencies.dependency.version' for org.slf4j:slf4j-api:jar is missing.

Spring Boot nutzt genau diesen BOM-Mechanismus aus 2️⃣.
Das Spring Boot Parent-POM bringt ein BOM mit, das alle Versionen vorgibt.


## Transitive Dependencies:

The dependencies of dependencies are called transitive dependencies. This can go infinate. maven pulls in all the dependencies of dependencys of dependencys etc. it is like a dependency tree.

## Other remote repositorys
we can tell maven about other remote repositorys that it can use. This can be convenient, when we need a dependency that cannot be found in the maven central repository. 


To do this, we need to edit our settings.xml file. Go to the directory where maven is installed. then go to the conf folder. this folder holds all the configuration. in this folder, the settings.xml file should be. changes in there will affect all the maven instances installed on the machine.

place a new profile into the profiles section and activate it:

<profiles>
 <profile>
 <id>spring_remote</id>
 <repositories>
 <repository>
 <id> spring_repository
 <url> http://repo.spring.io/release/
  </repository>
   </repositories>
    </profile>
    </profiles>

<activeProfiles>
<activeProfile>spring_remote</activeProfile>
</activeProfiles>

Now, maven can work with the spring remote repository as well.

## Was ist ein classpath?
Der Classpath ist einfach gesagt der Suchpfad, den die Java Virtual Machine (JVM) benutzt, um Klassen und Ressourcen zu finden (also .class-Dateien, Konfigurationsdateien, Properties, usw.).

Wenn dein Java-Programm läuft und du z. B. schreibst:

import org.slf4j.Logger;

…dann muss die JVM wissen, wo diese Klasse org.slf4j.Logger liegt.
Genau das steht im Classpath.

## Scopes:

dependency scope tells us about when a dependency will be available during mavens lifecycle. 

in each dependency you can define a scope:

<dependencies>
        <dependency>
            <groupId>io.micronaut</groupId>
            <artifactId>micronaut-bom</artifactId>
            <version>4.4.2</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>

if you do not specify any scope tag in a dependency, then the scope is "compile". So "compile" is the default scope.

**compile** scope means:
dependencies are available on all classpaths. so they are available both when we build the application and at runtime. they are available during the build-, test- and run-phases of our project.

**test** scope means:
the dependency is not required for building or executing the project. it is required for executing our unit test. Only when we execute our unit tests, then we will see these dependencies on our classpath.

**provided** scope means:
when you expect the dependency to be provided by sth. like the JDK or the container you are using. The dependency will be available for the build and test phases but when we deploy (or "jar-up") our application, those dependency will not be available in that package because something else will be providing them.

**runtime** scope means:
dependency is available when we execute the system or when we test the application. But NOT when we compile the application. An example for that is the jdbc driver implementation. it is not needed until you actually run the application. 

**system** scope means:
dependency will be provided by the file system. you place your jar files in a folder on the file system and maven goes out and finds the dependency on a file path that you specify. This can be really dicey because there are different paths on different machines. It makes your application rigid and normally is not recommended.

**import** scope means:
import indicates that this dependency should be replaced with all effective dependencies declared in its POM. The import scope is rarily used.



## Excluding transitive dependencies in Maven

At its core, to exclude Maven dependency means to prevent a specific transitive dependency from being included in your project's classpath. When you declare a dependency in your pom.xml, Maven automatically pulls in not only that direct dependency but also all its own dependencies, and so on, creating a dependency tree. While usually beneficial, this can lead to issues:

1. Dependency Conflicts: Different versions of the same library being pulled in by various transitive paths, leading to runtime errors or unpredictable behavior. It is not uncommon that you have a dependency, that has transitive dependencies which conflict with another dependency's transitive dependencies.

2. Project Bloat: Unnecessary libraries being included, increasing JAR size and potentially impacting application startup time or memory footprint.

3. Security Vulnerabilities: Including vulnerable versions of libraries through transitive paths that are not directly needed.


Here’s a common example of how to exclude Maven dependency within a pom.xml:

<dependency>
    <groupid>com.example</groupid>
    <artifactid>parent-library</artifactid>
    <version>1.0</version>
    <exclusions>
        <exclusion>
            <groupid>com.unwanted</groupid>
            <artifactid>unwanted-transitive-dependency</artifactid>
        </exclusion>
    </exclusions>
</dependency>

In this snippet, parent-library might transitively bring in unwanted-transitive-dependency. By using the  tag, we explicitly tell Maven to prevent this specific transitive dependency from being included 

It's also possible to exclude Maven dependency specifically within Maven plugins. This is useful when a plugin itself introduces an undesirable dependency. The syntax is similar, but it resides within the plugin's configuration:

<build>
    <plugins>
        <plugin>
            <groupid>org.apache.maven.plugins</groupid>
            <artifactid>maven-compiler-plugin</artifactid>
            <version>3.8.1</version>
            <dependencies>
                <dependency>
                    <groupid>org.apache.maven.shared</groupid>
                    <artifactid>maven-shared-utils</artifactid>
                    <version>0.2</version>
                    <exclusions>
                        <exclusion>
                            <groupid>commons-io</groupid>
                            <artifactid>commons-io</artifactid>
                        </exclusion>
                    </exclusions>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>

## Snapshot Versions

e.g. <version>1.0.0-SNAPSHOT</version>

A snapshot version in Maven is one that has not been released.

The idea is that before a 1.0 release (or any other release) is done, there exists a 1.0-SNAPSHOT. That version is what might become 1.0. It's basically "1.0 under development". This might be close to a real 1.0 release, or pretty far (right after the 0.9 release, for example).

The difference between a "real" version and a snapshot version is that snapshots might get updates. That means that downloading 1.0-SNAPSHOT today might give a different file than downloading it yesterday or tomorrow.

Usually, snapshot dependencies should only exist during development and no released version (i.e. no non-snapshot) should have a dependency on a snapshot version.

When you build an application, Maven will search for dependencies in the local repository. If a stable version is not found there, it will search the remote repositories (defined in settings.xml or pom.xml) to retrieve this dependency. Then, it will copy it into the local repository, to make it available for the next builds.

For example, a foo-1.0.jar library is considered as a stable version, and if Maven finds it in the local repository, it will use this one for the current build.

Now, if you need a foo-1.0-SNAPSHOT.jar library, Maven will know that this version is not stable and is subject to changes. That's why Maven will try to find a newer version in the remote repositories, even if a version of this library is found on the local repository. However, this check is made only once per day. That means that if you have a foo-1.0-20110506.110000-1.jar (i.e. this library has been generated on 2011/05/06 at 11:00:00) in your local repository, and if you run the Maven build again the same day, Maven will not check the repositories for a newer version.

Maven provides you a way to change this update policy in your repository definition:

// Source - https://stackoverflow.com/a
// Posted by Romain Linsolas, modified by community. See post 'Timeline' for change history
// Retrieved 2025-11-27, License - CC BY-SA 4.0

<repository>
    <id>foo-repository</id>
    <url>...</url>
    <snapshots>
        <enabled>true</enabled>
        <updatePolicy>XXX</updatePolicy>
    </snapshots>
</repository>

where XXX can be:

    always: Maven will check for a newer version on every build;
    daily, the default value;
    interval:XXX: an interval in minutes (XXX)
    never: Maven will never try to retrieve another version. It will do that only if it doesn't exist locally. With the configuration, SNAPSHOT version will be handled as the stable libraries.

also: snapshot version bedeutet: es wird ständig geschaut, ob es neuen code gibt, der in dieser version drinnen ist. Das heißt bei snapshot versionen kann sich der code ständig updaten. 

## Maven <dependencyManagement> vs. <dependencies> Tags

<dependencyManagement> allows to consolidate and centralize the management of dependency versions without adding dependencies which are inherited by all children. This is especially useful when you have a set of projects (i.e. more than one) that inherits a common parent.

Another extremely important use case of dependencyManagement is the control of versions of artifacts used in transitive dependencies. 

Hier eine genaue Erklärung: https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html

## Cyclic Dependencies

In Maven bezeichnet “cyclic dependency” (zyklische Abhängigkeit) eine Situation, in der sich zwei oder mehr Module gegenseitig direkt oder indirekt als Abhängigkeit benötigen – ein Abhängigkeitskreis.

🔄 Beispiel für eine direkte zyklische Abhängigkeit

Modul A hängt von B ab

Modul B hängt von A ab

→ A → B → A

🔄 Beispiel für eine indirekte zyklische Abhängigkeit

A hängt von B ab

B hängt von C ab

C hängt wiederum von A ab

→ A → B → C → A

❗ Warum ist das ein Problem?

Maven kann solche Kreise nicht auflösen, weil es die Reihenfolge nicht bestimmen kann, in der die Artefakte gebaut werden müssen.

Das Build bricht normalerweise ab mit einer Fehlermeldung wie “Failed to collect dependencies…”.

Es führt zu schwer wartbaren und stark gekoppelten Modulen.


## Lifecycles and Phases

a lifecycle helps us when we build, test or distribute an artifact. The lifecycle consists of a set of steps or stages that we go through as we build our artifact. These stages/steps are referred to as phases. Within maven, there are 3 built in lifecycles.
- default
- clean
- site

these lifecycles have their own particular phases. When a lifecycle is executed, then a goal will bind to a particular phase. In general a goal is an action that is going to be taken. A goal is like a method that performs some kind of action against our project. 

execute the clean phase of the **clean lifecycle**: "mvn clean"
what will happen is that the clean phase will be invoked as well as every phase that precedes the clean phase within the clean lifecycle.

Der Clean-Lifecycle von Maven besteht aus genau drei Phasen:

1. pre-clean
2. clean
3. post-clean

Wenn du mvn clean ausführst, wird die Phase clean sowie alle ihr vorausgehenden Phasen im Clean-Lifecycle ausgeführt.

➡️ Ausgeführt werden also: pre-clean und clean.

Die Phase post-clean wird nicht ausgeführt, weil sie nach clean kommt.

Give us some information about the clean phase:
"mvn help:describe -Dcmd=clean"

**default lifecycle** is one of the most important lifecycles. It has 23 phases and it will adjust itself depending upon the package you specify within your pom.xml file ( <packaging>jar</packaging> ).

Give us some information about the deploy phase:
"mvn help:describe -Dcmd=deploy"

These are some of the most important phases out of the 23 default lifecycle phases:

- compile
- test-compile
- test
- package
- install
- deploy

let's take a look at them in detail:

**Compile Phase** (compile the source code of the project)
the compile phase is going to compile all of the source code within our "src/main/java" directory and then it turns our java files into .class files which we then can execute via the jvm.

**Test-Compile phase** (compile the test source code into the test destination directory)
Diese Phase funktioniert ähnlich wie die compile phase nur dass sie diesmal unsere unit tests unter "src/test/java" compiled.

**test phase** (Run tests using a suitable unit testing framework. These tests should not require the code be packaged or deployed)

This phase is used when we are running our particular unit tests. This phase is going to kick off those tests. By default maven will stop the build if any of the tests fails but it can be configured to ignore failures or to skip unit tests all together.

for example: pom.xml konfiguration to turn off tests within maven:
<build>
    <plugins> 
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <configuration>
                <testFailureIgnore>true</testFailureIgnore>
            </configuration>
        </plugin>
    </plugins>
</build>

**package phase** (take the compiled code and package it in its distribute format, such as a JAR)

This phase is when we take our compiled code and we build that artifact that we are going to distribute. So we are talking about the JARs and WARs. Those items that have our code packaged up that we can then deploy to a server or we can use within another code-base. The package phase will create exactly this artifact.

**install phase** (install the package into the local repository, for use as a dependency in other projects locally)

takes the artifact that is created within the package phase and places it within the local repository. That allows other maven projects to depend on that project or we can reference that new artifact using the coordinates in a variety of other ways.

**deploy phase** (Done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects)

This phase is about working across the environments. You can have a remote repository (e.g. maven central) or maybe your organization has created its own remote repository. When we deploy, we basically push our artifact to that remote repository that can be used for integration purposes amongst developers.

## Plugins and goals

Goals perform tasks that are ran against our projects in order for something to happen. Goals are like methods in a class. 

Plugins are comprised of a number of goals. A plugin is like a class that has methods. So a plugin has many goals. 

Syntax for executing a plugin:
mvn [pluginName]:[goal]

e.g. mvn compile:compile

where do these plugins come from? Because Plugins are available even if i do not specify them in my pom.xml file.
Answer: those plugins come from the Super-POM.

wenn man sich das effective pom ansieht, dann kann man die ganzen plugins sehen:
mvn help:effective-pom

Folgendes zeigt nähere Informationen über das compiler plugin an:

mvn help:describe -Dplugin=compiler

## Plugin Properties

we can change the behaviour of a plugin goal by specifying some arguments. Each plugin goal has some properties. And these properties allow us to adjust how the goal behaves. 
If we wanna see the different properties that can be used on a particular plugin goal, we can use the help plugin:

mvn help:describe -Dcmd=compiler:compile -Ddetail

we then get a list of different properties we can set for this goal.

How can we set those properties?

1. We can set it via commandline:
for example setting the verbose property on the compile goal of the compiler plugin:
mvn compiler:compile -Dmaven.compiler.verbose=true 

2. We can set it via plugin configuration in pom.xml file:
in this case, the properties will be applied when we invoke the plugin goal directly or when the goal is invoked by a lifecycle phase. This is a better way to capture the behaviour that we would like our project build to perform:

<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <verbose>true</verbose>
                </configuration>
            </plugin>
        </plugins>
    </pluginManagement>
</build>

when we then execute the compile goal on the compiler plugin:

mvn compiler:compile

it will execute the verbose flag as a trigger

## Creating your own maven custom plugins

use the archetype plugin to create a project for our custom plugin. You also have to tell the plugin which groupId and artifactId the new plugin should have. 

mvn archetype:create 
    -DgroupId=com.infiniteskills.maven
    -DartifactId=first-custom
    -DarchetypeArtifactId=maven-archetype-mojo
    -DarchetypeGroupId=org-apache.maven.archetypes

Archetype create is deprecated! Use Archetype:generate instead!

weitermachen bei 2:40:00

## Annotation processors
## Dependency Mediation

## Optional Dependencies


Quellen:

https://www.vervecopilot.com/interview-questions/how-can-mastering-how-to-exclude-maven-dependency-elevate-your-professional-interviews

https://www.youtube.com/watch?v=uAQs-YXnY-U 

https://www.baeldung.com/maven-dependency-scopes

https://stackoverflow.com/questions/5901378/what-exactly-is-a-maven-snapshot-and-why-do-we-need-it

https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Management