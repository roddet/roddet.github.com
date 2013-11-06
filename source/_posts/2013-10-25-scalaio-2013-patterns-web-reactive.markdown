---
layout: post
title: "Scala IO 2013 => Patterns de développement pour une application web réactive et concurrente"
date: 2013-11-06 09:30
comments: true
categories: scalaio2013
---

## Présentateurs
{% img left /images/scalaio2013/speaker-fabrice-croiseaux.png %}
Fabrice Croiseaux, CEO chez InTech Luxembourg. Il anime régulièrement des podcasts [NipDev](http://www.niptech.com/podcast/category/nipdev/).

* Twitter : [@fXzo](https://twitter.com/fXzo)
* LinkedIn : [http://www.linkedin.com/in/fcroiseaux](http://www.linkedin.com/in/fcroiseaux)
* Github : [fcroiseaux](https://github.com/fcroiseaux)
* Sa bio sur [Scala IO](http://scala.io/speakers/fabrice-croiseaux.html)
<br>
{% img left /images/scalaio2013/speaker-antoine-detante.png %}
Antoine Detante, Ingénieur chez InTech Luxembourg.

* Twitter : [@antoined](https://twitter.com/antoined)
* LinkedIn : [http://www.linkedin.com/in/fcroiseaux](http://www.linkedin.com/in/fcroiseaux)
* Github : [adetante](https://github.com/adetante)
* Sa bio sur [Scala IO](http://scala.io/speakers/antoine-detante.html)

<br><br>
=> La description de [la session ScalaIO](http://scala.io/events/patterns-de-developpement-pour-une-application-web.html)

## Le Talk !
Antoine et Fabrice sont arrivés 2nd au concours [Typesafe’s Developer Contest](http://typesafe.com/blog/developer-contest-winners-announced) qui a eu lieu en début d'année.

Ils ont concouru avec une application _Car Race Dashboard_ construite avec [Play! Framework](http://www.playframework.com/) qui offre en temps réel la visualisation d'un _dashboard_ de course de voitures.

Ce talk a été l'occasion pour eux de présenter leur travail.

{% img center /images/scalaio2013/web-reactive-1.jpg %}


## Une application construite avec 3 composants principaux

* _Race Simulation_ => un moteur qui simule le déplacement de voitures

* _Race Dashboard_ => Stockage des positions dans une base [Mongo DB](http://www.mongodb.org/) + calcul des statistiques en asynchrone + notification des résultats

* _Web Client_ => un client web qui affiche en temps réel l'évolution de la course

## _Race Simulation_

### Du KML en guise de circuit
Pour simuler le déplacement d'une voiture, il faut déterminer une liste de coordonnées possibles. 
Cette liste de coordonnées est commune à toutes les voitures lors d'une course.
Elle dépend évidemment du circuit sélectionné pour la course.

La liste des points est obtenue à partir d'un fichier [KML](http://en.wikipedia.org/wiki/Keyhole_Markup_Language) pour un circuit. A titre d'exemple, vous pouvez consulter celui du Mans [ici](http://www.comintech.net/kml/LeMans.kml).
Ce fichier _XML_ sera parsé pour récupérer la liste des coordonnées possibles pour une course donnée.

Curieux de voir à quoi ressemble le code Scala qui a en entrée une URL de fichier [KML](http://en.wikipedia.org/wiki/Keyhole_Markup_Language) et en sortie une liste de points ?

``` scala TrackParser https://github.com/intechgrp/CarRaceDashboard/blob/master/app/simulation/TrackParser.scala
def readTrack(url: String): List[TrackPoint] = readTrack(XML.load(url))

def readTrack(data: Elem): List[TrackPoint] = {
	val positions = (for {
	  pos <- (data \\ "coordinates").text.split("\n")
	  if (pos.trim.length > 0)
	} yield pos.trim).toList.map {
	  pos =>
		val coordinates = pos.split(",")
		Position(coordinates(1).toDouble, coordinates(0).toDouble)
	}
	positions.zipWithIndex.map(v => TrackPoint(v._2, v._1))
}
```

### Des acteurs, des acteurs, des acteurs

* _Race Actor_ => Acteur qui représente la course et qui permet de lancer toutes les voitures via un événement _start_. Il va aussi se charger de communiquer l'évolution des voitures au composant _Race Dashboard_.

* _Car Actor_ => Acteur qui simule le déplacement d'une voiture. 
Une fois l'événement _start_ reçu du _Race Actor_, il va déplacer à intervalle régulier une voiture suivant la liste des points ordonnés du circuit.

[Code source des acteurs](https://github.com/intechgrp/CarRaceDashboard/blob/master/app/simulation/Race.scala)

## _Race Dashboard_

### _Storage Actor_
Cet acteur va sauvegarder les vitesses, positions et les distances successives dans la base [Mongo DB](http://www.mongodb.org/). 
Le driver [Reactive Mongo](http://reactivemongo.org/) y est utilisé.

[Code Source de l'acteur](https://github.com/intechgrp/CarRaceDashboard/blob/master/app/models/StorageActor.scala)

### _Stats Actor_
_Stats Actor_ calcule à partir des données stockées :

* La vitesse moyenne
* La vitesse maximale
* Le classement général

[Code Source de l'acteur](https://github.com/intechgrp/CarRaceDashboard/blob/master/app/models/StatsActor.scala)

### _RTEventListener_
Un acteur _RTEventListener_ est instancié pour chaque client Web. Il écoute les messages du composant des acteurs _Storage Actor_ et _Stats Actor_ puis notifie via [SSE](http://en.wikipedia.org/wiki/Server-sent_events) le client web.

## _Web Client_
Le client Web est construit avec :

* [Coffescript](http://coffeescript.org/) qui s'intègre naturellement avec [Play! Framework](http://www.playframework.com/)
* [backbone.js](http://backbonejs.org/) pour une structure MVC
* [Twitter Bootstrap](http://getbootstrap.com/) pour la présentation générale de l'application
* [Kineticjs](http://kineticjs.com/) pour le dessin du circuit et le déplacement des voitures

## A quoi ressemble l'application ?

{% img center /images/scalaio2013/web-reactive-2.png %}

## Les slides de la présentation
<iframe src="http://www.slideshare.net/slideshow/embed_code/27621039" width="597" height="486" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>

## Ce que j'en ai pensé
Ce talk montre un exemple d'architecture se reposant complètement sur [le modèle d'acteur](http://fr.wikipedia.org/wiki/Mod%C3%A8le_d'acteur). 
Ce style d'architecture permet une organisation séparant clairement les responsabilités entre les acteurs et une communication asynchrone entre les différents composants.

Je me suis poser la question de l'intérêt de l'instanciation d'un acteur _RTEventListener_ par client Web. Je pensais trouver 1 acteur unique pour communiquer via [SSE](http://en.wikipedia.org/wiki/Server-sent_events) avec tous les clients. Je comprends qu'en utilisant 1 acteur par client Web, on se donne l'opportunité de notifier de façon concurrente les clients.

Asynchronisme + séparation des responsabilités + concurrence constituent-ils les fondations d'une application web réactive ? 

Vous souhaitez en savoir plus sur ce style d'architecture ? Consulter le code sur [Github](https://github.com/intechgrp/CarRaceDashboard/) et surtout inscrivez-vous au cours gratuit [_Principles of Reactive Programming_](https://www.coursera.org/course/reactive) de Coursera.