---
layout: post
title: "Scala IO 2013 => De Maven à SBT"
date: 2013-10-28 09:30
comments: true
categories: scalaio2013 scala maven sbt
---

## Présentateur
{% img left /images/scalaio2013/speaker-stephane-manciot.jpg %}
Stéphane Manciot, Architecte chez ebiznext.

* Twitter : [@smanciot](https://twitter.com/smanciot)
* LinkedIn : [http://www.linkedin.com/in/smanciot](http://www.linkedin.com/in/smanciot)
* Sa bio sur [Scala IO](http://scala.io/speakers/stephane-manciot.html)
* La description de [sa session ScalaIO](http://scala.io/events/de-maven-a-sbt.html)

<br><br><br>
## Une photo comme si vous y étiez !

{% img center /images/scalaio2013/stephane-manciot-maven-sbt-1.jpg %}


## Le Talk
Stéphane nous a défini ce qui serait pour lui un outil de build idéal, c'est à dire un outil qui _facilite la vie des développeurs_. Ce talk fait suite à une migration de Maven à SBT d'un projet entièrement écrit en Java. Oui pas de code Scala dans le projet !

Deux idées principales dans ce _talk_ :

### Partie 1 => SBT c'est mieux que Maven
SBT et Maven seront comparés sur les thèmes suivants :

* Plugins Maven vs tâches SBT
* Phases Maven vs Gestion des dépendances des tâches SBT
* Performances
* Gestion multiprojets
* Cross compilation pour les projets Scala
* Gestion des dépendances

### Partie 2 => On peut faire du SBT en Entreprise

Voici les préconisations de Stéphane :

* Configurer 2 catégories de dépôt en entreprise : Ivy et Maven.
* Une solution pour publier les versions _snapshots_ et _releases_ (voir le code SBT dans les slides)
* Une publication dans Sonar avec le plugin [sbt-sonar](https://github.com/ebiznext/sbt-sonar) avec une couverture du code via le plugin [jacoco4sbt](https://bitbucket.org/jmhofer/jacoco4sbt)


## Les slides de la présentation
<iframe src="http://www.slideshare.net/slideshow/embed_code/27564746" width="600" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>

## Ce que j'en ai pensé
Une présentation intéressante qui met en évidence les différences de philosophie entre Maven et SBT. Pour ceux qui utilisent Maven au quotidien, elle montre les défauts de Maven qui ne sont pas toujours visibles lorsqu'on ne connait pas d'autres outils de build.

On pourra néanmoins regretter le manque d'objectivité lors de la comparaison Maven vs SBT. En effet, aucun défaut de SBT n'est cité dans les slides et à mon avis il y en a quand même :) J'ai le sentiment que la migration de Maven vers SBT de quelques projets Java chez ebiznext a surtout été motivé par la volonté d'harmoniser le système de build avec les projets scala de l'entreprise. Comme le confirmera la séance de questions/réponses, Gradle n'a par exemple pas été envisagé comme remplaçant éventuel de Maven.

Un dépôt Git est disponible pour illustrer ce talk [maven2sbt](https://github.com/ebiznext/maven2sbt). 

J'ai lancé les 2 builds (Maven et SBT). Le build SBT est Ok, j'ai eu une erreur avec build Maven (Maven 3.1.1 et commande _mvn clean install_)

{% img center /images/scalaio2013/stephane-manciot-maven-sbt-2.png %}

Quoi qu'il en soit, c'est une très bonne initiative surtout que ce build traite des problématiques de génération de code, de compilation de code groovy, etc...







