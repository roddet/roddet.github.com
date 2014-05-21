---
layout: post
title: "Soirée Devoxx France 2014 au JUG Nantes"
date: 2014-05-20 08:41
comments: true
categories: devoxxfr14 jugnantes
---

Ce mardi 20 mai, c'était soirée Devoxx France avec le [JUG Nantes](http://nantesjug.org/#/).

Le contenu :

* Un tour d'horizon de _Devoxx France 2014_ avec [Thibaud Raison](http://nantesjug.org/#/speakers/thibaud_raison) et [Pierre Cosson](http://nantesjug.org/#/speakers/pierre_cosson).
* La session _Devoxx France 2014_ [_Gradle ne fait pas que remplacer Maven_](http://cfp.devoxx.fr/devoxxfr2014/talk/UGD-950/Gradle%20ne%20fait%20pas%20que%20remplacer%20Maven) rejouée par [Cédric Champeau](https://twitter.com/CedricChampeau). 

## Tour d'horizon de Devoxx France 2014
[Thibaud](http://nantesjug.org/#/speakers/thibaud_raison) et [Pierre](http://nantesjug.org/#/speakers/pierre_cosson) ont fait un résumé de leurs parcours _Devoxx_, donné leurs avis sur différentes sessions.

{% img center /images/nantesjug/mai14/retour-devoxx-france.jpg %}

Ils ont identifié 4 thèmes principaux : Java 8, Javascript, Docker et Big Data. Les _Keynote_ ne les ont pas vraiment marqué.

[Thibaud](http://nantesjug.org/#/speakers/thibaud_raison) et [Pierre](http://nantesjug.org/#/speakers/pierre_cosson) ont noté quelques sessions.

Est notée 4/4 :

* [Java 8 Streams & Collectors : patterns, performances, parallélisation](http://cfp.devoxx.fr/devoxxfr2014/talk/DNY-501/Java%208%20Streams%20&%20Collectors%20:%20patterns,%20performances,%20parall%C3%A9lisation)

Sont notées 3/4 :

* [JavaScript, the next big ... bytecode](http://cfp.devoxx.fr/devoxxfr2014/talk/DLD-453/JavaScript,%20the%20next%20big%20...%20bytecode)
* [Promesses et Yield : les Future<?> de JavaScript](http://cfp.devoxx.fr/devoxxfr2014/talk/ZSJ-347/Promesses%20et%20Yield%20:%20les%20Future%3C%3F%3E%20de%20JavaScript)
* [Construire une application réelle de jeux en ligne avec NoSQL](http://cfp.devoxx.fr/devoxxfr2014/talk/CDH-803/Construire%20une%20application%20r%C3%A9elle%20de%20jeux%20en%20ligne%20avec%20NoSQL)
* [http://cfp.devoxx.fr/devoxxfr2014/talk/MBI-023/Mise%20en%20production%20continue%20-%201%20an%20apr%C3%A8s](http://cfp.devoxx.fr/devoxxfr2014/talk/MBI-023/Mise%20en%20production%20continue%20-%201%20an%20apr%C3%A8s)

Sont notées 2/4 :

* [30 minutes pour développer une appli Java EE ? C'est largement suffisant avec Forge 2.0!](http://cfp.devoxx.fr/devoxxfr2014/talk/UGH-991/30%20minutes%20pour%20d%C3%A9velopper%20une%20appli%20Java%20EE%20%3F%20C%27est%20largement%20suffisant%20avec%20Forge%202.0!)
* [50 nouvelles choses que l'on peut faire avec Java 8](http://cfp.devoxx.fr/devoxxfr2014/talk/DWS-586/50%20nouvelles%20choses%20que%20l%27on%20peut%20faire%20avec%20Java%208)
* [Lambda Architecture - Choose your tools for Real-Time Big Data](http://cfp.devoxx.fr/devoxxfr2014/talk/WFL-752/Lambda%20Architecture%20-%20Choose%20your%20tools%20for%20Real-Time%20Big%20Data)
* [Web performances, regardons les résultats de près](http://cfp.devoxx.fr/devoxxfr2014/talk/TYU-863/Web%20performances,%20regardons%20les%20r%C3%A9sultats%20de%20pr%C3%A8s)

Le bilan Devoxx ? 

{% blockquote Thibaud Raison et Pierre Cosson au JUG Nantes %}
Après Devoxx France, on est motivé, on repart avec plein d'idées et des trucs à tester.
{% endblockquote %}

Et l'année prochaine ?

_Devoxx France 2015_ ça sera du 8 au 10 avril au Palais des Congrès. 1800 personnes sont attendues.

[Thibaud](http://nantesjug.org/#/speakers/thibaud_raison) et [Pierre](http://nantesjug.org/#/speakers/pierre_cosson) mettront à disposition leurs slides sur [le site du JUG Nantes](http://nantesjug.org/#/events/2014_05_20) très prochainement.

## Un petit mot sur Maven avant d'aborder la session de Cédric

Jusqu'à l'année dernière, je ne prenais pas très au sérieux les alternatives à Maven.
En effet, Maven est un des _super-héros_ du développeur Java.

Il a encouragé :

* La modularisation. Les projets sont devenus de plus en plus modulaires.
* Une normalisation (organisation des répertoires, cycle de vie d'un build, exécution des tests, etc.)
* Un essor de l'intégration continue

Au regard de tous ces services rendus, beaucoup de développeurs Java sont très attachés à Maven et sont donc beaucoup moins réceptifs aux alternatives.
J'étais un de ces développeurs.
Ca fait presque 8 ans que j'utilise Maven et presque autant de temps à en dire du bien.
J'ai même fait de [la promotion en Afrique lors de JCertif 2012](http://www.slideshare.net/RossiOddet/maven-par-la-pratique-14995676).

Il y a 3 ans, lorsque [Grégory Boissinot](https://twitter.com/gboissinot) présentait Gradle à ce même JUG Nantes.
Je n'étais pas réceptif et en plus le projet manquait de stabilité (modification trop fréquente de l'API).

Je me disais :

{% blockquote %}
C'est quoi cette chose qui ne veut pas rentrer dans le standard !
{% endblockquote %}

Je m'imaginais dans l'armée et je voyais les personnes qui faisaient autre chose comme des déserteurs.

J'étais allergique aux alternatives à Maven parce qu'elles :

* donnaient de la flexibilité au _standard_ Maven. Je me disais : Flexibilité = Entorse à la Norme = Désordre = Instabilité = Déserteurs :)
* pouvaient utiliser des langages dynamiques. J'estimais perdre en lisibilité et du temps à apprendre un nouveau langage.
* avaient une communauté restreinte et donc peu de plugins, réponses StackOverflow, etc.

Avec l'émergence des technologies frontend, des langages fonctionnels, des langages dynamiques, j'ai eu l'occasion de voir d'autres systèmes de _Build_ ([Gradle](http://www.gradle.org/), [Grunt](http://gruntjs.com/), [SBT](http://www.scala-sbt.org/), [Gant](http://gant.codehaus.org/), etc.).
Et [Maven](http://maven.apache.org/) me parait, désormais, vieilli face à ses concurrents.

Le projet [Gradle](http://www.gradle.org/) a bonne presse en ce moment.
Il a été bien aidé par des projets comme [Hibernate qui l'a adopté](https://community.jboss.org/wiki/Gradlewhy?_sscc=t).
L'année dernière, [Google l'a choisi comme système de build pour Android Studio](http://www.infoworld.com/t/development-tools/google-positions-gradle-the-build-system-of-choice-android-218852).

C'est donc en mode _réceptif_ que j'ai participé à la session de [Cédric](https://twitter.com/CedricChampeau).

## _Gradle ne fait pas que remplacer Maven_

{% img center /images/nantesjug/mai14/gradle-devoxx.jpg %}

Je vous invite à parcourir les slides de [Cédric](https://twitter.com/CedricChampeau) qui se lisent bien.

<script async class="speakerdeck-embed" data-id="846b09f0a93a0131565b426f673016e8" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

Parmi tous les avantages de Gradle par rapport à Maven, deux choses m'ont interpellé :

* Le Wrapper. Pour un projet donné, vous pouvez générer un wrapper (`gradle init`) qui permettre à tous les utilisateurs de récupérer la version de Gradle à utiliser pour le projet. 
C'est particulièrement pratique pour l'intégration continue et aussi pour changer de version de gradle d'un projet.
Cela permet aussi de faire cohabiter simplement différentes versions de l'outil de build chez un développeur.

* Pour réutiliser de la configuration de Build, Maven utilise l'héritage et Gradle utilise la composition.
C'est sur des points comme celui là que [Maven](http://maven.apache.org/) montre sa vieillesse.
Il a été conçu à une époque où l'héritage était le roi pour mutualiser du code. Les choses ont changé depuis... mais pas Maven...

Une chose est sûre, pour passer de [Maven](http://maven.apache.org/) à [Gradle](http://www.gradle.org/), il faut apprendre Gradle.
[Maven](http://maven.apache.org/) a ses défauts qui sont connus et maitrisés.
Il n'y a aujourd'hui aucune surprise à démarrer un projet avec [Maven](http://maven.apache.org/).
Est-ce que les experts [Maven](http://maven.apache.org/) sont prêts à prendre des risques ? Apprendre une nouvelle technologie alors que [Maven](http://maven.apache.org/) fonctionne ?

Noter l'existence du projet [Maven Polyglot](https://github.com/takari/maven-polyglot) qui va permettre de faire de la configuration Maven avec plusieurs langages : clojure, ruby, scala, yaml, et même Groovy :)
Est-ce une réponse suffisante ?
