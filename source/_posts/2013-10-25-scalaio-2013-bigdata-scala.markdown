---
layout: post
title: "Scala IO 2013 => Big Data + Scala"
date: 2013-11-07 09:30
comments: true
categories: scalaio2013
---

## Présentateurs
{% img left /images/scalaio2013/speaker-sam-bessalah.jpg %}
Sam Bessalah, indépendant intervenant sur des sujets tournant autour du _Big Data_, _Machine Learning_...

* Twitter : [@samklr](https://twitter.com/samklr)
* Sa bio sur [Scala IO](http://scala.io/speakers/sam-bessalah.html)
* La description de [la session ScalaIO](http://scala.io/events/big-data-plus-scala.html)
<br><br>
## Big Data + Analytics

{% img left /images/scalaio2013/big-data-scala-1.jpg %}
_What is Big Data Analytics ?_

C'est par cette question que commence le talk de Sam. La réponse sera :

{% blockquote %}
It's about doing aggregations and running complex models on large datasets, offline, in real time or both.
{% endblockquote %}

Pour analyser de gros volumes de données distribuées, il y a donc 3 stratégies :

* _offline_ (mode batch), 
* en temps réel,
* ou les deux en même temps.

Sam va tout au long de la session présenter les outils du _Big Data_ et leurs utilisations avec Scala. Il précisera pour chaque outil les stratégies compatibles.

## Hadoop + Scala
[Hadoop](http://hadoop.apache.org/) est un système permettant de créer des applications scalables massivement distribuées.
Dans le domaine du Big Data, il est utilisé en mode batch et non en temps réel.

Pour traiter de grands volumes de données distribuées, [Hadoop](http://hadoop.apache.org/) possède un module appelé _Hadoop MapReduce_ qui applique le pattern [_Map Reduce_](http://fr.wikipedia.org/wiki/MapReduce).

Sam a pris le temps d'expliquer le concept de _Map Reduce_ par des schémas et un exemple de code Scala.

Pour définir des _jobs Hadoop MapReduce_ en scala, Sam conseille l'utilisation de la librairie [Scalding](https://github.com/twitter/scalding).

## Storm + Scala
[Storm](http://storm-project.net/), contrairement à Hadoop, va permettre de traiter massivement les données en temps réel.

Pour utiliser [Storm](http://storm-project.net/) avec un DSL Scala, il y a le projet [ScalaStorm](https://github.com/velvia/ScalaStorm).

## SummingBird => Scala
[SummingBird](https://github.com/twitter/summingbird) est une librairie Scala qui fournit une abstraction du pattern _Map Reduce_ applicable à [Hadoop](http://hadoop.apache.org/) et à [Storm](http://storm-project.net/).

Ainsi, vous définissez vos jobs avec [SummingBird](https://github.com/twitter/summingbird) et vous allez pouvoir les exécuter dans [Hadoop](http://hadoop.apache.org/) ou [Storm](http://storm-project.net/).

## Apache Spark + Scala
[Apache Spark](http://spark.incubator.apache.org/) est une alternative au module _Hadoop MapReduce_ qui permet de faire des traitements distribués. 

Il est réputé plus performant que _Hadoop MapReduce_ et reste compatible avec [Apache Hadoop](http://hadoop.apache.org/).

Le concept clé de [Spark](http://spark.incubator.apache.org/) est de permettre le traitement simplifié de collections de données distribuées.
Il dispose d'une API en Scala, Java et Python donnant la possibilité d'enchainer des actions (count, collect, save, ...) ou encore des transformations (map, filter, groupBy, join, ...).

[Spark](http://spark.incubator.apache.org/) s'utilise en mode batch et non en temps réel.

## Apache Spark Streaming + Scala
[Apache Spark Streaming](http://spark.incubator.apache.org/docs/latest/streaming-programming-guide.html) étend les possibilités de [Spark](http://spark.incubator.apache.org/) en traitant des _streams_ de données en temps réel.

## Les slides de la présentation
<script async class="speakerdeck-embed" data-id="02c383b01f9901312c660a36078c81b4" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

## Ce que j'en ai pensé
Ce talk m'a permis de découvrir l'univers du Big Data. Scala y semble bien implanter :

* Existance de multiples API pour manipuler les systèmes les plus utilisés
* Des projets comme [Spark](http://spark.incubator.apache.org/) ou [SummingBird](https://github.com/twitter/summingbird) sont écrits en scala.

La programmation fonctionnelle (_FP_) semble trouver avec le Big Data un créneau d'utilisation particulièrement adapté. En effet, effectuer des analyses de données consiste souvent à appliquer un enchainement de transformations à ces données. Et la _FP_, de part sa nature, est l'outil idéal pour représenter un enchaînement de fonctions :)
