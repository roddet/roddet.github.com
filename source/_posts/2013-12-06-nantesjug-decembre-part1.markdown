---
layout: post
title: "Soirée JUG Nantes : Java 8 -> Lambdas, Streams et Collectors (Partie 1)"
date: 2013-12-06 21:41
comments: true
categories: jugnantes jugnantesdec13
---

Java 8, difficile de trouver mieux comme thème pour une soirée d'un JUG.
JUG qui, pour rappel, veut dire _Java User Group_ (Groupe d'utilisateurs de Java).
Ces derniers temps, les JUGs se sont diversifiées jusqu'à avoir des soirées qui n'ont rien (ou peu) à voir avec Java.
Ce n'est pas la faute aux JUGs, c'est vrai que les actualités IT _sexy_ viennent souvent d'ailleurs ;)

Mais ce 4 décembre à 19h à l'[EPITECH Nantes](http://www.epitech.eu/nantes/ecole-informatique-nantes.aspx), c'était place à un sujet phare de l'écosystème Java :

{% blockquote %}
La sortie imminente de Java 8 !
{% endblockquote %}

Avant de parler de Java 8, la soirée commence par un autre sujet : 
{% blockquote %}
Grails dans les tranchées
{% endblockquote %}
Ce _quickie_ est animé par [Dominique Jocal](https://twitter.com/djocal), Architecte Logiciel et Responsable de 2 domaines applicatifs chez [CBP Solutions](http://www.cbp-group.com/).

## _Grails dans les tranchées_ ? Mais avant... c'est quoi Grails ?
[Grails](http://grails.org/) est un framework de création d'applications web s'exécutant sur la [JVM](http://en.wikipedia.org/wiki/Java_virtual_machine). Le langage de développement est [Groovy](http://groovy.codehaus.org/), un Java _sucré_.

Il fait partie de la famille des frameworks dits _productifs_ qui ont les caractéristiques suivantes :

* _**Full Stack**_ -> Ces frameworks viennent avec toutes les briques applicatives nécessaires pour répondre aux problématiques courantes du web (communiquer avec une base de données, écrire des règles métiers, créer des pages web, gérer l'authentification, etc.).
* _**Simple**_ -> tout est fait pour faciliter le travail du développeur. Un mode opératoire simple est fourni pour mettre en place les fonctionnalités récurrentes du web.
* _**Web Friendly**_ -> Avec ces frameworks, vous arrêtez de vous battre avec le Web et en particulier le protocole HTTP. Vous saurez simplement exposer des services de type _REST_, gérer proprement les codes erreurs HTTP, etc.

Le précurseur dans ce domaine est [Ruby On Rails](http://rubyonrails.org/) créé en 2005 qui a fait la promotion au passage du langage Ruby.

Depuis, plusieurs frameworks sont apparus dans les autres écosystèmes :

* [CakePhp](http://cakephp.org/) pour Php.
* [Django](https://www.djangoproject.com/) pour Python.
* [Play! Framework](http://www.playframework.com/) pour Java ou Scala.
* [Sails](http://sailsjs.org/) pour Javascript.
* etc.

[Grails](http://grails.org/) complète cette liste de frameworks qui ont pour objectif principale la simplification des applications web.
C'est un produit open source qui tourne sur la [JVM](http://en.wikipedia.org/wiki/Java_virtual_machine).

Pour ceux qui évoluent dans l'écosystème Java, pas de surprise avec [Grails](http://grails.org/). 
Il est construit à l'aide de la boite à outils [Spring Framework](http://projects.spring.io/spring-framework/). Il s'agit d'ailleurs de la même compagnie derrière : [Pivotal](http://www.gopivotal.com/). 

[Grails](http://grails.org/) possède son propre serveur web et il est possible de générer un package WAR pour un déploiement dans un autre serveur d'application.

## Grails chez CBP
[Dominique](https://twitter.com/djocal) vient avec ce _Quickie_ présenter un retour d'expérience de l'utilisation de [Grails](http://grails.org/) chez [CBP Solutions](http://www.cbp-group.com/).

C'est devant une salle pleine qu'il commence à présenter le contexte [CBP Solutions](http://www.cbp-group.com/).

{% img center /images/nantesjug/dec13/nantesjug-dominique-jocal.jpg %}

[CBP Solutions](http://www.cbp-group.com/) c'est :

* Des centaines d'applications [IBM RPG](http://en.wikipedia.org/wiki/IBM_RPG)
* 60 applications Java
* 6 applications [Grails](http://grails.org/)

{% blockquote Dominique Jocal %}
CBP ce n'est pas du "Big Data" mais du "Big Rules" !
{% endblockquote %}

Les enjeux de [CBP Solutions](http://www.cbp-group.com/) sont les mêmes que pour la grande majorité des entreprises : il y a plus de codes que de données :) Non j'exagère c'est vrai. Seulement tout le monde n'a pas les mêmes problématiques que Facebook ou Twitter en terme de scalabilité sur le traitement de données. Le grand défi de la plupart des entreprises est de créer facilement des applications et surtout pouvoir effectuer la maintenance du code grandissant avec le temps.

## Le _Domain_ plus de métiers, plus de responsabilité
Les applications web de gestion consistent la plupart du temps à manipuler des données. Ces données sont souvent stockées dans des bases de données relationnelles. Elles sont alors stockées dans des tables. Afin de récupérer ces données dans l'univers applicatif et pouvoir aisément les manipuler, les développeurs sont amenés à créer des classes du _Domain_. Par exemple, si l'application consiste à manipuler des données d'une personne, ces données seront stockées dans une table _Personne_ et une classe _Personne_ sera créée pour encapsuler les données d'une personne (nom, prénom, etc.).

Il y a quelques années, il était _très tendance_ d'avoir des classes du _Domain_ sans aucune règle métier.
Cela avait du sens, il était question de construire des applications monolithiques qui répondaient à tous les besoins des entreprises.
Il fallait avoir des classes de _Domain_ les plus neutres possibles car elles devaient s'adapter à tous les contextes des différents endroits de l'applicatif. Elles étaient utilisées dans différents cas métiers.

De nos jours, la _tendance_ est plutôt de créer de multiples applications de petites tailles. 
Cela donne les avantages suivants :

* Une petite application est plus facile à maintenir. Son périmètre métier est identifié, c'est plus facile de l'apprendre, de le comprendre.
* Il devient plus simple dans un _SI_ (Système d'Information) d'identifier une brique applicative en erreur et l'origine des erreurs. En effet, les entrées/sorties d'une application de petite taille sont maitrisées et le développeur est dans des conditions idéales pour reproduire des erreurs dans son environnement de développement.
* Chaque application a son cycle de vie. Il n'est plus indispensable de _re-packager_ l'ensemble pour une évolution qui concerne un seul module. Plus besoin de redémarrer toutes les applications pour la mise à jour d'un libellé d'une application particulière.

En construisant des applications de périmètre fonctionnel réduit, le besoin d'avoir des classes _Domain_ les plus génériques possibles ne se posent plus. Il devient alors naturel de concentrer les besoins métiers dans ces classes. 

Quel meilleur endroit pour préciser que le nom d'une personne est obligatoire que dans la classe _Domain_ _Person_ ?

C'est dans cette logique que [Dominique](https://twitter.com/djocal) explique que le classes du _Domain_ doivent être moins anémiques. Elles vont porter les logiques métiers dont elles sont soumises. 

[Grails](http://grails.org/) apporte un support de validation déclaratif dans les classes de _Domain_.
Les différentes contraintes de validation des données sont déclarées directement dans ces classes.
[Grails](http://grails.org/) fournit aussi un cadre de développement facilitant l'écriture des tests des classes du _Domain_.

## La _Pizza Team_
Le principe de la _Pizza Team_ consiste à créer des équipes de petite taille (idéalement 1 pizza suffit à nourrir l'équipe) qui sont responsables de l'ensemble de l'application (du code à la base de données). 

Voici donc à quoi pourrait ressembler votre équipe :

{% img center /images/nantesjug/dec13/pizza-team.jpg %}

Chez [CBP Solutions](http://www.cbp-group.com/), ces principes sont appliqués, à la différence qu'il y a une équipe garante de la cohérence et l'urbanisation des données. 

Les différentes équipes projet qui travaillent sur des applications [Grails](http://grails.org/) co-conçoivent la base de données avec les garants des données du _SI_. 

## _Code first_ au lieu de _Schema first_
[Grails](http://grails.org/) donne la possibilité de générer un schéma de base de données à partir des classes de _Domain_. Les développeurs ont pu montrer aux administrateurs des bases de données qu'un schéma généré par [Grails](http://grails.org/) est de bonne qualité.

## Et l'IDE ? Ooopss, il en faut un pour développer ?
Les développeurs de [CBP Solutions](http://www.cbp-group.com/) sont globalement déçus de [GGTS](http://grails.org/products/ggts) : le _Groovy/Grails Tool Suite_. Il s'agit d'un [Eclipse](http://eclipse.org/) _re-packagé_ avec des plugins pour Groovy et [Grails](http://grails.org/).

Quelques mésaventures :

* Il arrive des compilations de code Groovy qui prennent beaucoup de temps pour se terminer en _OutOfMemoryError_.
* Des besoins fréquents de redémarrage de l'IDE et de l'application [Grails](http://grails.org/)
* Il est nécessaire d'avoir des PCs puissants (i7, 8GB de RAM, SSD).

Il y a des premiers retours positifs de l'IDE [Intellij IDEA](http://www.jetbrains.com/idea/).

## Grails -> Un développeur opérationnel tout de suite !
Chez [CBP Solutions](http://www.cbp-group.com/), avec [Grails](http://grails.org/), un développeur a un environnement de développement opérationnel rapidement en 3 étapes depuis son IDE :

* Récupération des sources d'un dépôt [Subversion](http://subversion.apache.org/).
* Raffraichissement des dépendances (`grails refresh-dependencies`)
* Lancerment de l'application (`grails run-app`)

[Grails](http://grails.org/) vient avec un serveur embarqué, pas besoin d'installer un serveur particulier pour développer.

[Grails](http://grails.org/) permet simplement de séparer les configurations de production de celles de développement. Par exemple, votre application peut fonctionner en mode développement sur une base de données en mémoire comme [H2](http://www.h2database.com/html/main.html) et fonctionner en production avec une base de données [MariaDB](https://mariadb.org/).

## Multi-Page vs Single-Page
[Grails](http://grails.org/) offre un cadre de développement avancé pour les applications _Multi-Page_.

Il est possible de faire du _Single-Page_. Vous continuer à profiter des leviers de productivités côté _back-end_. Pour le _front-end_, cherchez du côté de l'univers Javascript (_Vanilla_ ou frameworks type AngularJS, EmberJS, etc.).

Chez [CBP Solutions](http://www.cbp-group.com/), les applications sont faites en _Multi-Page_.

## Les tests c'est bien !
Les applications [Grails](http://grails.org/) chez [CBP Solutions](http://www.cbp-group.com/) sont toutes dans le top 10 des applications ayant la meilleure couverture de code par les tests.
 

## CBP, la suite...
[Dominique](https://twitter.com/djocal) a énoncé les projets qu'il avait en tête :

* Partager les astuces dans un Blog
* [Grails](http://grails.org/)! [Grails](http://grails.org/)! [Grails](http://grails.org/)! [CBP Solutions](http://www.cbp-group.com/) continue avec [Grails](http://grails.org/) !
* Explorer le parallélisme et mettre en place des tests de code Javascript

## En définitif
[Dominique](https://twitter.com/djocal) a présenté le retour d'expérience au sein de [CBP Solutions](http://www.cbp-group.com/) de l'utilisation de [Grails](http://grails.org/). Il est surpris qu'il n'y ait pas un _tsunami_ de [Grails](http://grails.org/) dans les entreprises qui font de l'informatique de gestion. Il est pour lui inconcevable, aujourd'hui de partir sur un assemblage _maison_ de librairies (Spring + Hibernate + etc.). [Grails](http://grails.org/) propose un ensemble cohérent, productif, _clés en main_ pour construire des applications web, autant en profiter.

Une question a été posée [Dominique](https://twitter.com/djocal) : est-ce que le côté _dynamique_ de Groovy n'était pas un problème car moins d'erreurs sont détectées à la compilation ? (je reformule avec mes mots ;)).
[Dominique](https://twitter.com/djocal) va expliquer que ce risque est compensé par la grande couverture de code des applications permise par [Grails](http://grails.org/).

Les slides ne sont pas encore disponibles, je complèterai cet article dès leurs publications.

## Et la soirée ? Elle continue ?
La seconde partie de la soirée sera animée par [Jose Paumard](https://twitter.com/JosePaumard) avec sa session 

{% blockquote %}
Java 8 : Lambdas, Streams et Collectors -> le nouveau visage de l'API Collection
{% endblockquote %}

A suivre dans un prochain article.

