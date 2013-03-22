---
layout: post
title: "JUG Nantes : Traçabilité des architectures distribuées"
date: 2013-03-19 20:15
comments: true
categories: jugnantes nodejs javascript
---

Hier j'ai eu l'occasion d'être présent à la session qu'organisait le JUG Nantes sur le thème : traçabilité d'une architecture distribuée & un retour sur le code story 2013.
 
Les intervenants : Sébastien Prunier & Jérôme Creignou
 
Voici un petit résumé de ce que l'on peut retenir.
 
## Code Story 2013
La session a commencé par le retour sur le code story 2013 qui a consisté à réaliser une même application avec différentes technologies (Sébastien => Java & Jetty Embedded, Jérôme => Javascript & NodeJS).
J'en retiens surtout le panel d'outil utilisé :
 
* Pour Sébastien c'est du Jetty Embedded, JUnit & Heroku. On peut quand même noter que le choix de Jetty n'est pas un hazard. Travailler avec Jetty est une approche que je vois de plus en plus qui se rapproche du modèle de programmation de NodeJS ou Play Framework où l'on souhaite avoir un contrôle accru des éléments composants le protocole HTTP (qu'est-ce qu'il y a dans la requête, qu'est-ce que je met exactement dans le réponse ? etc...). Vous pouvez trouver le détail de ses choix techniques là : [http://sebprunier.wordpress.com/2013/01/10/code-story-concours-pour-devoxx-france-2013/](http://sebprunier.wordpress.com/2013/01/10/code-story-concours-pour-devoxx-france-2013/)
* Pour Jérôme, une particularité : tout le cycle de développement entièrement sur le cloud avec Javascript & NodeJS. Oui entièrement : du développement au déploiement. Voici les outils :

	* [Cloud 9](https://c9.io/)  pour le développement. Pour du Javascript, l'IDE offre un debugger javascript et même un terminal. On peut faire du Git en ligne de commande :)
	* [Travis CI](https://travis-ci.org/) pour l'intégration continue
	* [Heroku](http://www.heroku.com/) pour le déploiement
 

## Traçabilité d'une architecture distribuée avec NodeJS & MongoDB
Les intervenants ont présenté une application qu'ils ont réalisé en 15 jours qui permet de traçer les requêtes à travers un SI multi-couche et hétérogène.

Le principe :

* Modifier les applications pour injecter et propager un identifiant de requête généré
* Installer sur chaque machine un agent construit avec Node.js qui parse les logs suivant un pattern configurable. Chaque agent structure en JSON les logs collectées et les envoie à un serveur de consolidation.
* Un serveur de consolidation récupère les données des agents et persiste les infos dans une base MongoDB. Il met à disposition également une IHM pour visualiser les données et faire des statistiques.
Un particularité à noter avec Node.js, la stratégie de lecture des fichiers de logs. Les fichiers ne sont pas montés en mémoire en totalité. L'API est notifié par l'OS qu'il y a eu un changement dans les fichiers et envoie le différentiel, ce qui fait que la lecture du fichier se fait efficacement.

Panorama des technologies utilisées :

* [Knockout.js](http://knockoutjs.com/) 
* [Expressjs](http://expressjs.com/)
* [D3js](http://d3js.org/)
* [Underscorejs](http://underscorejs.org/)
* [Winston](https://github.com/flatiron/winston)
* [Less](http://lesscss.org/)
* [JSONStream](https://github.com/dominictarr/JSONStream)
* [Event Stream](https://github.com/dominictarr/event-stream)

Les axes d'améliorations qu'ils ont déjà identifiés :

* Passer de MongoDB à CoucheBase pour 2 raisons : l'interface d'administration plus facile à vendre que la ligne de commande & pour des meilleurs performances
* Améliorer l'interface UI, le rendre un peu plus responsive
* Etudier une intégration avec [Log Stash](http://www.logstash.net/)

Voici les slides :

<iframe src="http://www.slideshare.net/slideshow/embed_code/17381444" width="600" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> 

