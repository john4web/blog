+++
title = "Apache Maven in Depth"
date = ""
description = ""
+++

https://www.youtube.com/watch?v=uAQs-YXnY-U 


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

*dependency management*

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

## Transitive Dependency Conflict Resolution

it is not uncommon that you have a dependency, that has transitive dependencies which conflict with another dependency's transitive dependencies.

weiterschauen bei: 2:07:00
