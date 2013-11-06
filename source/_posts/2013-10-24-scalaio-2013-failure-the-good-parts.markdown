---
layout: post
title: "Scala IO 2013 => Failure : The Good Parts"
date: 2013-10-27 09:30
comments: true
categories: scalaio2013 scala
---

## Présentateur
{% img left /images/scalaio2013/speaker-viktor-klang.jpeg %}
Viktor Klang, Directeur de l'ingénierie chez [Typesafe](http://typesafe.com/).

* Twitter : [@viktorklang](https://twitter.com/viktorklang)
* Tumblr : [klangism](http://klangism.tumblr.com/)
* Sa bio sur [Scala IO](http://scala.io/speakers/viktor-klang.html)
* La description de [sa session](http://scala.io/events/launch-conference.html)

<br><br><br>

## La keynote du jour 1 de _Scala IO_
Cette session a constituée la Keynote du premier jour de l'événement.

{% img center /images/scalaio2013/viktor_klang_failure_good_part.jpg %}

## Failure - The Bad Parts
Viktor a commencé par nous présenter les mauvais côté d'un plantage d'un système. Il l'a illustré avec l'exemple du projet Ariane 5 dont l'objectif principal était de créer un lanceur de satéllites géostationnaires.
Le 4 juin 1996, ce projet qui représentait 10 ans de recherches, 7 milliards de dollars investis, s'est tristement fait remarqué lors d'un test d'un système de lancement "nouvelle génération" qui va entrainer la destruction d'une fusée quelques secondes après son décollage. On doit cet échec à une erreur de programmation qui va coûter près de 370 millions de dollars ce jour là.

Un plantage logiciel en production peut donc avoir des conséquences desastreuses tant pour l'entreprise et peut-être même pour les développeurs de la solution :)

## Pour quelles raisons un système plante ?

* Surchage du système (trop d'utilisateurs, système sous dimensionné, ...)
* Plantage logiciel
* Plantage matériel (CPU, RAM, HDD, ...)
* etc...

Les causes possibles de plantage sont nombreux et de différentes natures. Les plantages font partis de la vie d'un système autant les prévoir.


## Validation vs Failure
Viktor a mis l'accent sur les différences entre les tests que nous pouvons faire pour valider un système et un plantage.

Les différences mises en avant :

* La validation d'un logiciel est intentionnelle tandis qu'un plantage est involontaire
* On attend d'une validation des résultats alors qu'on attend d'un plantage une reprise du système 

## Que savons-nous vraiment des erreurs possibles de notre système ?

4 catégories :

* _Known-Knows_
* _Known-Unknows_ 
* _Unknown-Knowns_ 
* _Unknown-Unknows_ 

## Tester ou vérifier son système ?

Les tests sont utiles pour la catégorie _known-knowns_.

Les vérifications du système sont utiles pour les catégories : _Unkown-Unknowns_, _Known-Unknowns_ et _Unknown-Unknowns_.

Conclusion ? => Faites les 2 !

## Attention à la _Defensive Programming_ !
La peur d'une erreur inattendue de notre système pousse certains développeur à de la _defensive programming_ qui conduit à :

* une confusion dans la gestion des responsabilités
* des blocs _try...catch_ abusifs pour essayer de prévoir tous les cas d'erreurs possibles

## Comment gérer ses risques d'erreur avec le :) ?

* _Distribution_ => Ne pas mettre tous ses oeufs dans le même panier.
* _Replication & Failover_ => Maintenez plusieurs exemplaires de votre contenu.
* _CircuitBreakers_ => Mettre en place des moyens pour éviter de maintenir la pression sur les parties du système en erreur. Par exemple, si une base de données ne répond plus, il ne sert à rien de continuer à l'intérogger continuellement tant que le problème n'est pas résolu.
* _Supervisor_ => Votre système doit vous notifier lorsqu'il y a des erreurs.
* _Bulkheading_ => Cloisonner votre système.
* _Compartmentalization_ => Compartimenter votre système pour éviter des effets en cascade sur tout votre système lorsqu'un composant est en erreur.
* _Graceful degradation_ => prévoir des modes de fonctionnement alternatifs en cas de plantage.

## Faites des microservices !

{% img center /images/scalaio2013/failure_good_part_2.jpg %}


## Quelques citations du talk
Viktor a illustré ses propos avec de nombreuses citations, voici celles que j'ai notées :

{% blockquote Niels Bohr %}
An expert is a man who has made all the mistakes which can be made, in a narrow field.
{% endblockquote %}

{% blockquote Edsger Dijkstra %}
Program testing can be used to show the presence of bugs, but never to show their absence !
{% endblockquote %}

{% blockquote Leslie Lamport %}
A distributed system is one in which the failure of a computer you didn't even know existed can render your own computer unusable.
{% endblockquote %}

{% blockquote Mitch Hedberg %}
An escalator can never break: it can only become stairs. You should never see an Escalator Temporarily Out Of Order sign, just Escalator Temporarily Stairs. Sorry for the convenience.
{% endblockquote %}

## Résumé du talk par Viktor

{% img center /images/scalaio2013/failure_good_part_1.jpg %}

## Ce que j'en ai pensé
On a eu droit à un Viktor en forme qui maitrisait le flux de sa présentation, une session très intéressante sur la gestion des erreurs d'un système. 

Ses slides ont du contenu intéressant, je n'ai pas pu trouver des traces de leur publication à ce jour. Vu que Viktor refait cette présentation en format [_Webinar_ le 30 octobre prochain](http://typesafe.com/blog/tune-in-for-viktor-klangs-webinar), je pense qu'il les publiera après.

Cette session a été filmée, nous aurons probablement la possibilité de la revoir en ligne dans les prochaines semaines.

[_Edit 06/11/2013 Ci-dessous la vidéo du Webinar du 30 octobre_]

<iframe width="640" height="360" src="//www.youtube.com/embed/_XMRYkuaob4?feature=player_detailpage" frameborder="0" allowfullscreen></iframe>
