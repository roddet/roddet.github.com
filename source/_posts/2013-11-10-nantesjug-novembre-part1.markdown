---
layout: post
title: "Soirée JUG Nantes : Du SQL au NoSQL en moins d'une heure (Partie 1)"
date: 2013-11-10 09:30
comments: true
categories: jugnantes jugnantesnov
---

Le 4 novembre dernier, j'ai eu l'opportunité de participer à une soirée organisée par le [JUG Nantes](http://nantesjug.org/) sous le thème "Du SQL au NoSQL en moins d'une heure". 

## Une salle pleine !
La plus grande salle de l'[EPSI Nantes](http://www.epsi.fr/Campus/Campus-de-Nantes#) était un peu petite face à l'engouement suscité par l'événement.

C'est le premier événement JUG Nantes auquel j'assiste depuis la rentrée. Oui c'est avec beaucoup de regrêts que je n'ai pas pu être présent à la première soirée de cette saison 2013-2014 :) Je remarque aussi que le public a changé, il y a beaucoup plus d'étudiants comparé à la saison précédente. Ceci est probablement lié au fait que l'[EPSI Nantes](http://www.epsi.fr/Campus/Campus-de-Nantes#) accueille l'événement.

{% img center /images/nantesjug/nov13/nantesjug-nov13-salle-pleine.jpg %}

Le JUG de Nantes va nous offrir 2 sessions :

* 1 Quickie _Amélioration de la qualité du code par restriction du langage_
* 1 talk _Elastifiez votre application : du SQL au NoSQL en moins d'une heure_

## Le quickie _Amélioration de la qualité du code_
[Hugo Wood](https://twitter.com/mercury_wood/) va nous présenter son retour d'expérience sur l'amélioration de la qualité du code. Il a appris le langage Java principalement en autodidacte avec des livres et travaille récemment en société de service où il côtoie beaucoup de _legacy_ (les connaisseurs ne seront probablement pas surpris ;)).

{% img center /images/nantesjug/nov13/nantesjug-nov13-hugo-wood.jpg %}

## C'est quoi la qualité du code ?
Hugo a commencé par poser les bases de la qualité du code. 

Par quoi reconnaît-on un code de bonne qualité ? => Par sa maintenabilité. Et pour avoir un code maintenable, il faut qu'il soit modulaire et testable.

## Les méthodes privées => cacher la misère ?
Vous l'avez surement déjà remarqué, les méthodes privées d'une classe ont tendance à se multiplier avec le passage d'un collègue qui vous dit avec fierté que le code n'était pas lisible et qu'il a fait du _refactoring_. Il avait trouvé une méthode publique trop _grosse_, vous savez ce genre de méthode qui ne se termine pas avec 2 _scrolls_ de votre souris. Il a alors  extrait (merci les IDE) des parties de code de la méthode en plusieurs méthodes privées. 

La lisibilité de la méthode s'est peut-être améliorée mais a t-on vraiment progressé sur la maintenabilité après une telle opération ?

La réponse est Non. En effet, cette opération conduit généralement à 2 catégories de méthodes privées :

* 1 méthode qui n'interagit avec aucun champ de la classe. Que fait-elle alors dans cette classe ? Elle pourrait être _static_ votre code fonctionnerait toujours. Il convient donc de l'extraire dans une nouvelle classe.

* 1 méthode privée qui modifie la valeur d'un champ de la classe. Cela conduit souvent à perdre le contrôle rationnel de ce  champ. En effet, lorsque vous lirez votre méthode publique à _refactorer_, vous verrez l'appel à cette nouvelle méthode. Vous ne vous rendrez pas forcément compte qu'il modifie un champ et que la suite de votre algorithme dépend fortement de l'exécution de cette nouvelle méthode. Comme dit Hugo, un _parfum de variable globale_... La bonne option est donc d'extraire ce champ et cette méthode dans une nouvelle classe.

Etes-vous prêt à arrêter de cacher la misère en vous "privant" des méthodes privées ?

## L'héritage de classe => notre ennemi ?
Ceux qui travaillent au quotidien avec une grande volumétrie de code, l'admettront aisément : l'héritage de classe, quand il est mal utilisé pose quelques difficultés de maintenance :

* Comment tester une classe fille sans effet de bord de la classe mère ?
* Comment tester unitairement une classe abstraite sans ses classes concrètes ?
* Utiliser des champs _protected_ d'une classe mère dans une classe fille, une encapsulation à plusieurs vitesses ?

L'héritage de classe est malheureusement souvent utilisé, à tort, pour factoriser du code. Il est alors facile de se retrouver par exemple avec : 

* Une méthode dont l'exécution conduit à naviguer à travers plusieurs niveaux de la hiérarchie des classes
* Une méthode vide (avec un commentaire _// ne rien faire_) dans certaines classes pour un traitement à ne pas effectuer dans des cas particuliers

Hugo va préconiser l'utilisation des interfaces et de la composition pour traiter nos besoins courants. Il va l'illustrer en proposant un _refactoring_ possible de la classe abstraite _AbstractCollection_ du JDK avec une interface et la composition (illustration présente dans ses slides).

Etes-vous prêt à ne plus utiliser l'héritage de classe ?

## Les slides du Quickie

<iframe src="http://www.slideshare.net/slideshow/embed_code/27900604" width="700" height="569" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe>

Vous l'aurez compris, Hugo encourage une _restriction_ de l'utilisation du langage Java pour améliorer la qualité de du code. Il argumente que cela permettra aux débutants de commettre moins d'erreurs de _design_ et aux expérimentés d'agir avec un meilleur discernement.

## La suite ! La suite ! La suite !

Après ce quickie, [Tugdual Grall](https://twitter.com/tgrall) et [David Pilato](https://twitter.com/dadoonet) ont pris le relais pour le talk _Elastifiez votre application : du SQL au NoSQL en moins d'une heure_. A travers un exemple de code, ils vont nous montrer comment passer du SQL au NoSQL avec [CouchBase](http://www.couchbase.com/) et [Elasticsearch](http://www.elasticsearch.org/).

A suivre dans la 2ème partie de cet article dans les prochains jours...

