---
layout: post
title: "Scala IO 2013 => Pimp my GC - du Scala supersonique"
date: 2013-10-29 09:30
comments: true
categories: scalaio2013
---


## Présentateur
{% img left /images/scalaio2013/speaker-pierre-laporte.png %}
Pierre Laporte, consultant Java/Scala chez Xebia France.

* Twitter : [@pingtimeout](https://twitter.com/pingtimeout)
* Blog : [http://www.pingtimeout.fr](http://www.pingtimeout.fr)
* Sa bio sur [Scala IO](http://scala.io/speakers/pierre-laporte.html)
* La description de [sa session ScalaIO](http://scala.io/events/pimp-my-gc-du-scala-supersonique.html)

<br><br><br>

## Une photo souvenir

{% img left /images/scalaio2013/pierre-laporte-pimp-my-gc-1.jpg %}


## Il n'existe pas de tuning JVM miracle !
La session commence par une mise en garde => Ne copier/coller pas des recettes de tuning JVM sur internet, il n'y a pas de solution universelle. Les solutions exposées sur le web sont spécifiques aux problèmes rencontrés par les auteurs et ne sont surtout pas applicables à toutes les applications.

Un conseil de Pierre :

{% blockquote Pierre Laporte %}
Si vous ne comprennez pas un flag, ne l'utilisez pas !
{% endblockquote %}

## L'hypothèse générationnelle, c'est vrai !
Il s'agit d'une théorie énonçant que la plupart des objets meurent jeunes. Pierre va le mettre en évidence en comparant deux applications : 1 écrite en Java et 1 écrite en Scala. Ces 2 applications ont des consommations mémoires très différentes, en moyenne 48GB/j pour une et de l'ordre de 3 TB/j pour l'autre.

Laquelle des applications consomment 3 TB/j ? Java ou Scala ?

=> La réponse est dans ce cas Java... Bien évidemment, il s'agit de 2 applications complètement différentes et il n'est pas du tout question ici de dire que les applications Java consomment plus de mémoire que les applications Scala :)

Ce qu'il faut retenir, c'est que malgré cet écart de consommation mémoire, l'hypothèse générationnelle se vérifie pour les 2 applications, simplement à des échelles différentes.

## Deux zones mémoires pour la _Garbage Collector (GC)_ avant G1

* _Young Generation_ pour les objets jeunes
* _Old Generation_ pour les vieux objets

_Comment est déterminé l'age d'un objet ?_ => Au nombre de cycles du _Garbage Collector_ auxquels a survécu un objet.

## Le temps de _GC_ est proportionnel au nombre d'objets vivants !

Pierre nous a présenté ses expériences qui vont mettre en évidence quelque chose de très intéressant sur nos programmes tournant sur la JVM.

## Expérience N°1 
* Faire tourner un programme qui crée de nombreux objets qui meurent vites
* 50 GB de mémoire
* Un paramétrage de la _Young Generation_ à 49.9 GB

Nous avons là un paramétrage idéal du GC qui permet d'absorber la majorité des objets jeunes créés dans la _Young Generation_.

_Le GC a mis 6 ms pour nettoyer 38GB de mémoire_


## Expérience N°2
* Faire tourner un programme qui crée de nombreux objets qui meurent vites
* 50 GB de mémoire
* Un paramétrage de la _Young Generation_ à 10 MB

Nous avons là un paramétrage défavorable du _GC_ qui va faire passer nos objets très rapidement dans la _Old Generation_ faute de place dans la _Young Generation_.

_Le GC mettra 322 ms pour nettoyer 52GB de mémoire_

## Expérience N°3+
Pierre refait les expériences précédentes avec un programme qui générent beaucoup d'objets qui vivent longtemps et là les performances du _GC_ se sont révélées catastrophiques.

## L'immutabilité et le GC
Avec les résultats des expériences précédentes, on craint moins les conséquences du concept d'immutabilité des langages fonctionnels comme Scala. Le _GC_ nettoie aisément un grand nombre d'objets non référencés.

On peut quand même retenir comme conséquence de l'immutabilité, l'augmentation de la fréquence de passage du _GC_. Pierre nous propose dans ses slides, un tuning pour contrôler cette fréquence si jamais elle se révèle génante pour votre application.

## Comment tuner le _G1 (Garbage First) Garbage Collector_ ?

G1 est le nouvel algorithme poussé par Oracle depuis le JDK 7. La particularité avec cet algorithme est qu'il se paramètre "presque" tout seul.

Pierre nous propose d'observer les règles suivantes pour son tuning :

* Mettre la même valeur pour Xms et Xmx
* Supprimez toutes nos optimisations antérieures (-Xmn, -XX:TenuringThreshold, ...)
* Définir des intervalles de temps pour le _GC_ (-XX:MaxGCPauseMillis et -XX:GCPauseIntervalMillis)
* Activer les logs GC


## Les slides de la présentation

<iframe src="http://www.slideshare.net/slideshow/embed_code/27543319?rel=0" width="600" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>


## Ce que j'en ai pensé
Nous avons eu droit à une présentation de qualité, bien rythmée. Pierre a fait preuve de pédagogie pour expliquer de façon claire le fonctionnement des différents algorithmes du _Garbage Collector_ de la JVM.

Je me suis souvent inconsciemment inquiété des nombreux objets à durée de vie courte (de l'ordre de la méthode) que nous créons à travers nos programmes. Cette présentation me donne un nouveau regard sur ces objets :)

Présentation à revoir en vidéo pour ceux qui souhaitent avoir des éléments de compréhension du fonctionnement de la JVM d'Oracle.