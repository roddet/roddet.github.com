---
layout: post
title: "Scala IO 2013 => Our journey from UML/MDD to Scala macros"
date: 2013-11-05 09:30
comments: true
categories: scalaio2013
---

## Présentateur
{% img left /images/scalaio2013/speaker-hayssam-saleh.png %}
Hayssam Saleh, CTO chez Ebiznext.

* Twitter : [@hayssams](https://twitter.com/hayssams)
* LinkedIn : [http://fr.linkedin.com/in/hayssams](http://fr.linkedin.com/in/hayssams)
* Github : [hayssams](https://github.com/hayssams)
* Sa bio sur [Scala IO](http://scala.io/speakers/hayssam-saleh.html)
* La description de [sa session ScalaIO](http://scala.io/events/our-journey-from-umlmdd-to-scala-macros.html)

<br><br>
## Le Talk !
Ce talk nous présente un retour d'expérience concernant une migration UML vers des macros scala pour modéliser une application.

{% img center /images/scalaio2013/uml-scala-1.jpg %}

## Les inconvénients d'une modélisation basée sur UML

* Un cycle de vie peu efficace composé d'un enchainement de transformations : UML -> XMI -> Code -> ByteCode. Il fallait près de 4h pour avoir le livrable correspondant à une modification du modèle. L'étape la plus chronophage (78% du temps) était _XMI -> Code_.

* Pas de solution _exploitable dans un vrai projet_ pour effectuer des merges de modèles UML. Ce qui entrainait la mise en place d'un _lock_ (1 modification à la fois) pour éviter d'avoir à effectuer des merges.

* Le manque de représentation standard de certaines caractéristiques du modèle (Les génériques Java par exemple)

## Un DSL comme alternative
Hayssam et son équipe ont proposé alors de s'orienter vers l'utilisation d'un DSL qui doit avoir les caractéristiques suivantes :

* Compréhensible du _product owner_
* Compilable du développeur

## Des macros Slick
Les macros Scala utilisés sont basées sur [Slick](http://slick.typesafe.com/), un framework de persistance. Hayssam va nous présenter un exemple de _modélisation_ avec une macro Slick et comment se traduisent les définitions des :

* Entités => _case classes_
* Relations 0..1 => _Option_
* Relations 1..1 => Composition d'entités
* Relations n..n => _List_
* Contraintes => via l'utilisation de mots clés du DSL


## Un export UML
Les macros [Slick](http://slick.typesafe.com/) vont permettre de générer des diagrammes UML via l'exécution d'une ligne de code.

## La magie faite à la compilation, pas d'injection au runtime
Les macros [Slick](http://slick.typesafe.com/) vont impacter le code à la compilation, le bytecode généré contient déjà toutes les transformations. Cela permet de profiter de toute la puissance du compilateur et de profiter du typage fort de Scala.

Pas de magie au runtime => à l'exécution, seul le code compilé est exécuté.

[Slick](http://slick.typesafe.com/) permet aussi de contruire des requêtes qui vont profiter d'un typage fort facilitant la détection d'erreurs à la compilation. 

## Les slides de la présentation

<iframe src="http://www.slideshare.net/slideshow/embed_code/27545078" width="650" height="534" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>

## Ce que j'en ai pensé
Je suis arrivé un peu par hasard dans cette session. C'était en fin de journée et je m'étais en fait trompé de salle :) Cela dit, je suis content d'y avoir assisté.

Il est intéressant d'avoir des retours d'expérience d'utilisation de DSL dans le monde de l'entreprise. La modélisation qui constitue souvent naturellement la richesse documentaire des applications est ici faite sans induire la double maintenance habituelle (modèle + code). Nous échappons ici à la génération de codes que nous détestons la plupart du temps en tant que développeur.

Ce qui m'a manqué dans cette présentation, ce sont les impacts sur le plan humain et organisationnel de cette migration. J'imagine que des personnes qui étaient _habituées_ à modéliser via des _boites_ ont dû, au moins au début, manifester une certaine hostilité face à ce changement :) J'aurai voulu savoir s'il y avait eu une stratégie particulière pour créer de l'adhésion autour de ce nouveau paradigme.

Pour en savoir plus, le dépôt Github [https://github.com/ebiznext/slick-macros](https://github.com/ebiznext/slick-macros) contient les exemples du Talk et ce [Wiki](https://github.com/ebiznext/slick-macros/wiki) documente la solution proposée.
