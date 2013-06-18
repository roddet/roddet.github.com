---
layout: post
title: "BreizhCamp 2013 # Gatling"
date: 2013-06-13 18:00
comments: true
categories: breizhcamp13 gatling
---

J'ai eu la formidable occasion d'animer un Tools In Action : "Stressez vos applications web avec [Gatling](http://gatling-tool.org/)" à l'événement [BreizhCamp](http://www.breizhcamp.org/) édition 2013.

## Un souvenir

{% img center /images/breizhcamp13/gatling/breizhcamp_gatling.jpg %}

## Les points abordés

* Présentation de Gatling
* Démo # mise en oeuvre d'un test de charge sur la prochaine application de l'événement JCertif 2013 incluant :
	* Création d'un projet Maven dans [Scala IDE](http://scala-ide.org/) à partir de l'[archetype Gatling](http://repository.excilys.com/content/groups/public/archetype-catalog.xml)
	* Lancement du recorder depuis [Scala IDE](http://scala-ide.org/)
	* Un export HAR via Chrome
	* Génération d'une simulation à partir d'un fichier HAR via le recorder
	* Complément du scénario avec une requête POST mettant en oeuvre un [feeder](https://github.com/excilys/gatling/wiki/Feeders) et un template de corps de message
	* Exécution / Rapport d'exécution
* Présentation des extensions et de la communauté

Et le tout en 30 mn :)

## Les slides

[http://blog.roddet.com/breizhcamp-2013-gatling](http://blog.roddet.com/breizhcamp-2013-gatling)

## Le code

[https://github.com/roddet/breizhcamp-2013-gatling](https://github.com/roddet/breizhcamp-2013-gatling)
