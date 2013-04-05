---
layout: post
title: "Devoxx France 2013 # Tools in Action # Trucs et astuces avec Java et MongoDB sur Raspberry Pi"
date: 2013-04-05 13:54
comments: true
categories: raspberrypi java mongodb
---

Vous pouvez retrouvez la description de la session [sur le site de devoxx](http://www.devoxx.com/display/FR13/Trucs+et+astuces+avec+Java+et+MongoDB+sur+Raspberry+PI).

## Pourquoi j'ai choisi cette session ?
Une occasion de découvrir le Raspberry Pi.


## L'animateur

**Guillaume Scheibel** [@g_scheibel](https://twitter.com/g_scheibel) [www.gsheibel.net](http://www.gscheibel.net)

{% img center /images/devoxxfr13/raspberrymongo/speaker.jpg %}



## Raspberry Pi c'est quoi ?
{% blockquote Wikipedia http://fr.wikipedia.org/wiki/Raspberry_Pi %}
Le Raspberry Pi est un ordinateur monocarte à processeur ARM conçu par le créateur de jeux vidéo David Braben, dans le cadre de sa fondation Raspberry Pi2.
L'ordinateur a la taille d'une carte de crédit, il permet l'exécution de plusieurs variantes du système d'exploitation libre GNU/Linux et des logiciels compatibles. Il est fourni nu (carte mère seule, sans boîtier, alimentation, clavier, souris ni écran) dans l'objectif de diminuer les coûts et de permettre l'utilisation de matériel de récupération.
{% endblockquote %}

## Raspberry Pi pour faire quoi ? 

* Du linux
* Un serveur Git
* Un serveur [XBMC](http://xbmc.org/)
* Du code

En fait, à nous d'inventer les usages :)

## Les modèles de Raspberry Pi

### Modèle A (~ 25 euros)

* Processeur 700 MHz
* 1 port HDMI
* 1 slot SDCARD
* 1 port USB
* 256MB de RAM

### Modèle B (~ 35 euros)
En mieux par rapport au modèle A :

* 2 ports USB
* 512 MB de RAM
* 1 port Ethernet

## Attention il faut des accessoires !
Le Raspberry Pi est vendu dans le plus simple appareil. 

Pour votre première commande, il vous faudra en plus :

* un carte SD
* un cable d'alimentation (micro USB)
* un boitier
* des connectiques : HDMI, Ethernet, Clavier

## Vous cherchez des informations sur le Raspberry Pi ?
Le site [http://elinux.org/RPi_Hub](http://elinux.org/RPi_Hub) est là pour vous aider.

## Les distributions Linux
Il y a plus de 20 distributions Linux compatibles.

La distribution officielle est [Raspbian](http://www.raspbian.org/).

## Java
On a le choix entre OpenJDK vs Oracle. Le présentateur a choisi Oracle.
La version compatible est 1.8 et il y a la preview de JavaFX.

## Java & GPIO
Des API Java permettent d'intéragir avec les pins GPIO.

Il y a par exemple [Pi4J](http://pi4j.com/).

## MongoDB
Raspberry Pi n'est pas compatible avec les versions de Mongo officielles.

Un projet fait le portage de MongoDB sur Raspberry Pi : [https://github.com/skrabban/mongo-nonx86](https://github.com/skrabban/mongo-nonx86).

Un article permet de guider pas à pas à l'installation de MongoDB sur Raspberry Pi : [http://elsmorian.com/post/24395639198/building-mongodb-on-raspberry-pi](http://elsmorian.com/post/24395639198/building-mongodb-on-raspberry-pi)

Attention il faut compter 7 à 8 heures pour un build complet.

## Démonstration

Elle a consisté à présenter et exécuter un programme Java qui va lire dans une base mongodb des données (événements Devoxx France précedemment récupérés) et affiche le résultat.

## Les slides sur Parleys

[http://www.parleys.com/#play/515ac911e4b0ffdd7e058b9e](http://www.parleys.com/#play/515ac911e4b0ffdd7e058b9e)

<object width="600" height="395"><param name="movie" value="http://www.parleys.com/dist/share/parleysshare.swf"></param><param name="allowFullScreen" value="true"></param><param name="wmode" value="direct"></param><param name="bgcolor" value="#222222"></param><param name="flashVars" value="sv=true&pageId=515ac911e4b0ffdd7e058b9e" ></param><embed src="http://www.parleys.com/dist/share/parleysshare.swf" type="application/x-shockwave-flash" flashVars="sv=true&pageId=515ac911e4b0ffdd7e058b9e" allowfullscreen="true" bgcolor="#222222" width="395" height="395"></embed></object>

## Bilan
Une bonne introduction à Raspberry Pi avec une démonstration concrète de son utilisation.