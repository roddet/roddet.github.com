---
layout: post
title: "Devoxx France 2013 # Tools In Action # Gradle en 30 minutes pour tout changer"
date: 2013-04-04 13:52
comments: true
categories: devoxxfr2013 gradle
---
Vous pouvez retrouvez la description de la session [sur le site de Devoxx France](http://www.devoxx.com/display/FR13/Gradle%2C+30+minutes+pour+tout+changer).

## Pourquoi j'ai choisi cette session ?
Gradle est une alternative choisie par des grands acteurs comme les équipes d'Hibernate, de Grails etc...
Je souhaitais à travers cette session découvrir Gradle et surtout voir jusqu'à quel niveau de maturité était ce produit.


## Le présentateur
Sébastien Cogneau [@SCogneau](https://twitter.com/SCogneau) ([Bio](http://www.devoxx.com/display/FR13/Sebastien+Cogneau)) contribue le week end au projet Gradle et a fait migrer la plupart des projets d'un DSI à Gradle.

## Gradle en quelques mots
{% img left http://blog.roddet.com/images/devoxxfr13/gradle/gradle_logo.gif %}

* Un DSL en groovy
* Extensible
* Possibilité de réutiliser des tâches ANT
* Réutilise les conventions Maven
* Compatible avec Ivy et Maven
 
Le projet est composé de 60% de code java et 40% de code groovy

## Live Coding
La session consistait à passer de Maven à Gradle avec un projet exemple : [DDDSample](http://dddsample.sourceforge.net/).

### Etape 1 : Récupérer les sources du projet
Les sources sont disponibles [ici](http://dddsample.sourceforge.net/download.html) ou directement [là](http://sourceforge.net/projects/dddsample/files/latest/download?source=files) puis décompresser l'archive récupéré (dddsample-1.1.0-src.zip).

Test Maven
```
mvn install
```
```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```
Magnifique ce Maven :)

### Etape 2 : Création du fichier build.gradle
Il s'agit du fichier par défaut de Gradle, l'équivalent du pom.xml de Maven. Il est possible d'utiliser un fichier nommé autrement.

Voici une première configuration qui permet de compiler le projet avec ses dépendances et de générer un JAR.

``` groovy
apply plugin : 'java'

repositories {
   mavenCentral()
}

dependencies {
    compile 'org.springframework:spring-webmvc:2.5.6'
    compile 'org.springframework:spring-web:2.5.6'
    compile 'org.springframework:spring-jms:2.5.6'
    compile 'org.springframework:spring-jdbc:2.5.6'
    compile 'org.springframework:spring-orm:2.5.6'
    compile 'org.springframework:spring-aop:2.5.6'
    compile 'org.hibernate:hibernate-core:3.3.1.GA'
    compile 'org.slf4j:slf4j-api:1.5.6'
    compile 'org.slf4j:slf4j-log4j12:1.5.6'
    compile 'org.slf4j:jcl-over-slf4j:1.5.6'
    compile 'javassist:javassist:3.8.0.GA'
    compile 'org.hibernate:hibernate-annotations:3.4.0.GA'
    compile 'commons-lang:commons-lang:2.3'
    compile 'commons-io:commons-io:1.3.1'
    compile 'commons-dbcp:commons-dbcp:1.2.2'
    compile 'hsqldb:hsqldb:1.8.0.7'
    compile 'javax.servlet:servlet-api:2.5'
    compile 'taglibs:standard:1.1.2'
    compile 'javax.servlet:jstl:1.1.2'
    compile('org.apache.activemq:activemq-core:5.2.0') {
        exclude module: "commons-logging"
    }
    compile 'org.apache.xbean:xbean-spring:3.4.3'
    compile 'org.apache.cxf:cxf-rt-frontend-jaxws:2.1.3'
    compile 'org.apache.cxf:cxf-rt-transports-http:2.1.3'

    runtime 'opensymphony:sitemesh:2.3'

    testCompile 'org.easymock:easymock:2.3'
    testCompile 'org.springframework:spring-test:2.5.6'
    testCompile 'junit:junit:4.4'
}
``` 

### Etape 3 : lancement du build
```
gradle clean build
```  
Le clean n'est à appliquer que à la première compile, le reste du temps "gradle build" suffit.
Gradle offre une bonne gestion de la compilation incrémental.



### Etape 4 : Voir la liste des tâches disponibles
```
gradle tasks
```
Voici le résultat
```
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'dddsample-1.1.0'.
dependencyInsight - Displays the insight into a specific dependency in root project 'dddsample-1.1.0'.
help - Displays a help message
projects - Displays the sub-projects of root project 'dddsample-1.1.0'.
properties - Displays the properties of root project 'dddsample-1.1.0'.
tasks - Displays the tasks runnable from root project 'dddsample-1.1.0' (some of the displayed tasks may belong to subprojects).

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.

Rules
-----
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.
Pattern: clean<TaskName>: Cleans the output files of a task.

To see all tasks and more detail, run with --all.
```

### Etape 5 : Packaging WAR
Modifier le fichier "build.gradle" en remplaçant "apply plugin :'java'" par : 
```
apply plugin : 'war'
```
Ce changement suffit à générer un WAR lors d'un "gradle build".

### Etape 6 : Intégrer Jetty
Modifier le fichier "build.gradle" 
```
apply plugin:'jetty'
```
Lancer le serveur Jetty
```
gradle jettyRunWar
```
L'application est accessible via l'URL : http://localhost:8080/dddsample-1.1.0/

{% img center http://blog.roddet.com/images/devoxxfr13/gradle/dddsample.png 500 %}


### Etape 7 : Configurer sonar
Ajouter
``` groovy
...
apply plugin:'sonar-runner'
...
configurations{
    codeCoverage
}
...
sonarRunner{
    sonarProperties{
        property "sonar.jacoco.reportPath", "${buildDir}/codeCoverage.exec"
    }
}
...
dependencies {
	...
	codeCoverage group: 'org.jacoco', name: 'org.jacoco.agent', version: '0.6.2.201302030002', classifier: 'runtime'
}

test{
    jvmArgs "-javaagent:${configurations.codeCoverage.asPath}=destfile=${buildDir}/codeCoverage.exec,includes=se.citerus.*"
}

task wrapper(type:Wrapper){
	gradleVersion="1.5-rc1"
}
```
Générer le wrapper
```
gradle wrapper
```
Cette commande génère les exécutables (gradle,gradlew,gradlew.bat) pour s'interfacer avec jenkins. L'exécutable généré permet de dérouler un build sans explicitement installer Gradle sur une machine.

### Etape 8 : Intégrer à Jenkins

Le plugin [Gradle pour Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Gradle+Plugin) a été utilisé.

Deux façons de le mettre en oeuvre : 

* soit avec une installation sur la même machine que Jenkins et une configuration Système
* soit avec l'utilisation du wrapper et là aucun besoin d'installer Gradle

Le build est paramétré avec :
``` 
clean build sonarRunner
``` 
Malheureusement à cette étape...FAILED...impossible de charger une classe de Gradle. Le présentateur avait fait une mise à jour le matin même, surprise surprise... Mais nous avons compris le principe et l'effet démo est là :)

### Etape 9 : Créer une tâche
Il est facile de créer une tâche avec Gradle.

Exemple : 

``` groovy
task hello(type:Exec){
    description "tache pour devoxx 2013"
    group "devoxx"
    dependsOn wrapper
    executable "echo"
    args "Avez-vous des questions ?"
}
```
Grâce à l'attribut "dependsOn", on peut gérer l'ordonnancement des tâches. Ici "wrapper" va être exécuté avant "hello".

Lancer la nouvelle tâche :
```
gradle hello
```

On obtient :
```
:wrapper UP-TO-DATE
:hello
Avez-vous des questions ?

BUILD SUCCESSFUL

Total time: 5.781 secs
```

## Questions/réponses

### Quel niveau d'intégration avec les IDE ?

* [Spring Tools Suite](http://www.springsource.org/sts) Ok
* [Netbeans](https://netbeans.org/) Ok dans les version récentes
* [IntelliJ IDEA](http://www.jetbrains.com/idea/) offre la meilleur intégration

### Y a t-il un équivalent du plugin "release" de Maven ?
Non, pas d'équivalent de release plugin.

### Peut-on gérer des sous-modules ?
Oui

## Le code complet sur Github
[https://github.com/scogneau/devoxxfr2013](https://github.com/scogneau/devoxxfr2013)

## Bilan
Excellente session pour découvrir Gradle en 30 min chrono. Ca donne envie d'en savoir un peu plus.