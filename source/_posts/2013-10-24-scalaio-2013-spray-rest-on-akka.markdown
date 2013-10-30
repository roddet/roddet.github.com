---
layout: post
title: "Scala IO 2013 => Spray : REST on Akka"
date: 2013-10-30 09:30
comments: true
categories: scalaio2013
---

## Présentateur
{% img left /images/scalaio2013/speaker-mathias-doenitz.png %}
Mathias Doenitz, lead developpeur de [spray.io](http://spray.io/), commiteur sur le projet [Akka](http://akka.io/).

* Twitter : [@sirthias](https://twitter.com/sirthias)
* G+ : [Mathias Doenitz](https://plus.google.com/117715287005040955399)
* Sa bio sur [Scala IO](http://scala.io/speakers/mathias-doenitz.html)
* La description de [sa session ScalaIO](http://scala.io/events/spray-rest-on-akka.html)

<br><br><br>

## Le Talk
Ce talk a eu comme objectif la présentation de Spray, une couche HTTP pour les acteurs d'[Akka](http://akka.io/) à destination des développeurs Scala.

{% img /images/scalaio2013/mathias-doenitz-spray-io.jpg %}

## En Java nous avons déjà ce qu'il faut pour faire du HTTP
Pour communiquer à travers HTTP, nous disposons de multiples librairies et frameworks dans l'écosystème de la JVM (Servlets, Restlet, Spring MVC, JAXRS, ...). Scala étant basé sur la JVM, nous pouvons bien entendu "réutiliser" certains de ces outils. 

Mais...

## Les outils Java => un enfer pour le développeur Scala
Le développeur Scala est frustré car il y retrouve tous les défauts du monde Java qu'il a justement fui en faisant du Scala. 

Quels sont ces défauts que nous retrouvons généralement dans nos librairies/frameworks ?

* _Un conteneur Servlet_ avec sa session qui limite la scalabilité horizontale

* _XML, XML, XML,..._ Java étant un langage pas assez expressif pour être utilisé en DSL de façon convenable. Les configurations des outils Java reposent souvent sur d'autres formats XML, properties, ... là où les autres langages peuvent aisément s'utiliser en configuration. On pensera à [Groovy](http://groovy.codehaus.org/) avec la configuration d'un build [Gradle](http://www.gradle.org/) ou encore à Scala avec la configuration [SBT](http://www.scala-sbt.org/). C'est dommage de se retrouver avec des fichiers XML à configurer lorsqu'on travaille avec un langage pour lequel il possible de créer un DSL qui compile !

* _Des API et objets du modèle "mutables"_. En développant en _Scala Style_, vous vous débrouillez pour réaliser une application en prenant soin d'éviter d'utiliser des objets _mutables_ et vous voilà _condamné_ à utiliser une API qui ne respecte pas vos principes et qui peut vous entrainer des comportements difficiles à prévoir.

* _Les collections Java_. Scala fournit une API _riche_ pour manipuler des collections. Il est dommage de s'en priver.

* _Un typage limité_. Impossible de détecter des problèmes de configuration à la _compile_, trop de surprises au _runtime_.

## Que souhaite le développeur Scala ?

* Retrouver ses _case class_
* Communiquer en asynchrone via des messages (_Merci Akka_)
* Utiliser des _closures_, on est en 2013 quand même ! :)
* Consolider plusieurs requêtes asynchrones grâce aux _Futures_
* Manipuler _naturellement_ ses collections avec une API avancée

=> En somme, pouvoir pleinement profiter de Scala !

## Et voilà Spray !
Spray a pour objectif de réaliser les souhaits du développeur Scala en se reposant complètement sur Scala et Akka.

Spray est construit de façon modulaire pour nous donner la possibilité de n'utiliser que les parties qui nous intéressent. Il y a de nombreux modules : [spray-caching](http://spray.io/documentation/spray-caching/#spray-caching), [spray-can](http://spray.io/documentation/spray-can/#spray-can), [spray-client](http://spray.io/documentation/spray-client/#spray-client), [spray-http](http://spray.io/documentation/spray-http/#spray-http), [spray-httpx](http://spray.io/documentation/spray-httpx/#spray-httpx), [spray-io](http://spray.io/documentation/spray-io/#spray-io), [spray-servlet](http://spray.io/documentation/spray-servlet/#spray-servlet), [spray-routing](http://spray.io/documentation/spray-routing/#spray-routing), [spray-testkit](http://spray.io/documentation/spray-testkit/#spray-testkit), [spray-util](http://spray.io/documentation/spray-util/#spray-util), [spray-json](https://github.com/spray/spray-json).

Mathias a poursuivi son talk en présentant quelques modules de Spray :

* [spray-http](http://spray.io/documentation/spray-http/#spray-http) => structures de données HTTP basées sur les _case class_. Ce module permet entre autres de créer des objets _Requête_, _Réponse_. Le paramétrage HTTP est lui aussi typé, des _case class_ pour _Accept-Charset_, _Accept-Encoding_ etc...

* [spray-can](http://spray.io/documentation/spray-can/#spray-can) => API HTTP serveur et cliente de bas niveau construit sur [Akka IO](http://doc.akka.io/docs/akka/2.1.0/scala/io.html).

* [spray-routing](http://spray.io/documentation/spray-routing/#spray-routing) => ce module fournit un DSL avec des très nombreuses directives pour décrire :
	* le routage des requêtes
	* (Un)marshalling d'objets
	* la mise en cache des ressources statiques
	* la gestion des erreurs
	* ...

## A savoir !

* Spray est encore en cours de développement. A sa version finale, il changera de nom et s'appelera _akka-http_.
* [Play Framework](http://www.playframework.com/) va progressivement abandonner [Netty](http://netty.io/) pour _akka-http_.


## Les slides de la présentation

<iframe src="http://spray.io/scala.io/" width="800" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" allowfullscreen> </iframe>

Lien direct : [http://spray.io/scala.io/](http://spray.io/scala.io/)


## Ce que j'en ai pensé
Une bonne présentation d'un outil plein de promesses pour les développeurs Scala. Nous avons là une librairie qui va probablement devenir la référence dans l'écosystème de [TypeSafe](http://typesafe.com/).

L'abandon progressif de [Netty](http://netty.io/) au profit de _akka-http_ dans le projet [Play Framework](http://www.playframework.com/) montre une nouvelle fois la volonté de [TypeSafe](http://typesafe.com/) de créer un écosystème d'outils _fortement typés_ pour Scala et ne garder de Java que la JVM :)

On peut _légitimement_ se poser la question de la durée du support du langage Java dans les projets comme [Play Framework](http://www.playframework.com/) et [Akka](http://akka.io/) lorsque Scala aura _son_ écosystème suffisamment développé.

Si vous souhaitez en savoir plus, je vous recommande les liens suivants :

* [La philosophie de Spray](http://spray.io/introduction/what-is-spray/#philosophy), pourquoi il n'est pas conçu comme un framework
* [Des vidéos, slides, podcasts de l'équipe Spray](http://spray.io/introduction/other-resources/)
* Et bien sûr, la documentation officielle à travers [l'introduction](http://spray.io/introduction/) et [la référence](http://spray.io/documentation/).