---
layout: post
title: "BreizhCamp # Javascript du sol au plafond"
date: 2013-06-13 09:30
comments: true
categories: breizhcamp13 javascript nodejs mongodb
---

{% img center /images/breizhcamp13/js_sol_plafond/js-sol-plafond.jpg %}

## Animateurs

* [Sébastien Prunier](http://sebprunier.wordpress.com/)
* [Xavier Seignard](https://twitter.com/xavier_seignard)

## Le programme
L'objectif de ce Hands-on (3h) était de créer une application basée sur les technologies suivantes :

* MongoDB
* Node.js
* Angular.js

Et outillée par :

* Grunt
* Makefile

## Les supports

Les slides : [http://xseignard.github.io/breizhcamp-js/prez/](http://xseignard.github.io/breizhcamp-js/prez/)

Le code des différents exercices : [https://github.com/xseignard/breizhcamp-js](https://github.com/xseignard/breizhcamp-js)
 (1 tag par exercice).

## Le déroulement

Nous avons commencé la session par une distribution de clés USB contenant : les installeurs de Node.js, MongoDB, Git, un éditeur de texte, du dépôt Git des sources, ...

En regardant, les supports (slides et code), on voit qu'il y a eu un bon travail de préparation de cette session.

Les animateurs nous ont ensuite présenter MongoDB puis nous nous sommes lancés dans l'exercice 1 - Jouer avec MongoDB. Cela permetait aux participants de se familiariser aux opérations (insertion, recherche, etc...) sur une base de données MongoDB.

L'étape suivante a consisté à faire connaissance avec Node.js avec l'exercice 2 dont l'objectif était d'insérer des enregistrements en base de données depuis un fichier contenant des données au format JSON.

Il y a eu dans la salle de nombreux problèmes d'installation d'environnement au regard de nombreux outils à installer. Ces problèmes ont ralenti tout le groupe et ceux comme moi qui avait fini assez rapidement les exercices (les solutions étaient, à peu de choses près, décrites dans les slides), c'était un peu l'ennui malheureusement.

Pour arriver à la fin de l'exercice 2, j'ai compté environ 1h45 (sur les 3h). A ce moment, les animateurs se sont rendus compte qu'ils étaient en retard par rapport aux contenus prévus.

[Express.js](http://expressjs.com/) qui permet de réaliser des services REST est présenté et nous nous mettons à travailler sur l'exercice 3 - implémenter les services /geek et /geek/likes/:like?. Cette fois-ci, nous faisons plus efficace et en 30 min l'exercice est terminé.

Plus que 45 min et il reste encore beaucoup de points à voir. Le "Hands-on" en lui même s'arrête et on revient à une présentation rapide de ce qui a été prévu :

* L'utilisation de [Mocha](http://visionmedia.github.io/mocha/) pour faire des tests unitaires en javascript.
* AngularJS est à peine présenté par manque de temps, si le sujet nous intéresse, nous sommes invités à aller dans la session "Hands On AngularJS. Cette fois, vous allez coder !" prévu dans l'après-midi.
* L'outillage autour de javascript est présenté (Grunt, Bower, Istanbul et Makefile). A la différence de la plupart des "javascripteurs" qui font l'éloge de Grunt en tant qu'outil de build "ultime" (j'exagère un peu), ici Xavier affirme préférer faire un Makefile plutôt qu'utiliser Grunt. Ses arguments :
	* Le Makefile a déjà fait ses preuves et fait bien le boulot
	* Avec un Makefile on peut directement bénéficier des dernières versions de librairies alors qu'avec Grunt il faut attendre la mise à jour de la surcouche de la dite librairie.
* Les animateurs déconseillent de servir les fichiers statiques (CSS, JS etc...) par Node.js et conseille d'utiliser [Nginx](http://wiki.nginx.org/).


## Bilan

Les supports de cette session sont de qualités et nous avons tout ce qu'il nous faut pour refaire le Hands-On en dehors du BreizhCamp. C'est ce que je compte faire dès que j'aurai un peu de temps.

Je me suis malheureusement un peu ennuyé pendant une partie de la session car je n'avais rien à installer et j'avais déjà les bases de MongoDB et Node.js.

J'ai tout de même appris pas mal de chose :

* La mise en oeuvre de service REST avec [Express.js](http://expressjs.com/). Même si j'en avais déjà entendu parlé, je ne l'avais jamais expérimenté.
* L'utilisation de [async](https://github.com/caolan/async) pour éviter les enchainements de callback lors de multiples requêtes
* Le fait qu'il ne fallait pas servir les fichiers statiques avec Node.js et qu'il fallait penser à autre chose comme [Nginx](http://wiki.nginx.org/)
* Que le monde des "javascripteurs" était désespéré au point d'utiliser un Makefile pour effectuer un build, impensable dans le monde des "javaistes" :)







